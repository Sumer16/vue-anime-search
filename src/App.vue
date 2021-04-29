<template>
  <div>
    <header>
      <h1>Vue<strong>Anime</strong></h1>
      
      <form class="search-box" v-on:submit.prevent="handleSearch">
        <input type="search" class="search-field" name="" id="" placeholder="Search for an Anime..." v-model="search_query" required />
      </form>
    </header>
    <main>
      <div class="cards" v-if="animeList.length > 0">
        <Cards v-for="anime in animeList" :key="anime.mal_id" :anime='anime' />
      </div>
      <div class="no-results" v-else>
        <h3>Sorry, no results found 🔍...</h3>
      </div>
    </main>
  </div>
</template>

<script>
import { ref } from 'vue';
import Cards from './components/Cards';

export default {
  components: {
    Cards
  },
  setup() {
    const search_query = ref('');
    const animeList = ref([]);

    const handleSearch = async () => {
      animeList.value = await fetch(`https://api.jikan.moe/v3/search/anime?q=${search_query.value}`)
                              .then(res => res.json())
                              .then(data => data.results)
      search_query.value = '';
    };

    return {
      Cards,
      search_query,
      animeList,
      handleSearch
    }
  }
}
</script>

<style lang="scss">
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Fira Sans', sans-serif;
}
a{
  text-decoration: none;
}
header{
  padding-top: 50px;
  padding-bottom: 50px;
  h1{
    color: #888;
    font-size: 42px;
    font-weight: 400;
    text-align: center;
    text-transform: uppercase;
    margin-bottom: 30px;

    strong{
      color: #313131;
    }
    &:hover{
      color: #313131;
    }
  }

  .search-box {
    display: flex;
    justify-content: center;
    padding-left: 30px;
    padding-right: 30px;
    align-items: center;

    .search-field{
      appearance: none;
      background: none;
      border: none;
      outline: none;
      background-color: #f3f3f3;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);

      display: block;
      width: 100%;
      max-width: 600px;
      padding: 15px;
      border-radius: 10px;
      color: #313131;
      font-size: 1.2rem;
      transition: 0.4s;

      &::placeholder{
        color: #aaa;
      }

      &:focus, &:valid {
        color: #fff;
        background-color: #313131;
        box-shadow: 0 0 0 rgba(0, 0, 0, 0.15);
      }
    }
  }
}

main{
  max-width: 1200px;
  margin: 0 auto;
  padding-left: 30px;
  padding-right: 30px;

  .cards{
    display: flex;
    flex-wrap: wrap;
    margin: 0 -8px;
  }
}

.no-results{
  height: 40vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
