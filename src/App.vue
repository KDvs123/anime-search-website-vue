<template>
  <div class="app">
    <header>
      <h1>The <strong>Anime</strong>Database</h1>
      <form action="" class="search-box" @submit.prevent="handleSearch">
        <input
          type="search"
          class="search-field"
          placeholder="Search for an anime..."
          required
          v-model="search_query"
        />
      </form>
    </header>

    <main>
      <div class="cards" v-if="animeList.length >0">
        <Card v-for="anime in animeList" :key="anime.mal_id" :anime="anime"/>
      </div>
      <div class="no-results" v-else>No Result</div>
    </main>
  </div>
</template>

<script>
import Card from './components/Card.vue'
import {ref} from 'vue';

export default {
  
  components: { Card },
  
  
  setup(){
    const search_query=ref("");
    const animeList=ref([]);

    const handleSearch= async()=>{
      animeList.value=await fetch(`https://api.jikan.moe/v4/anime?q=${search_query.value}`)
      .then(res => res.json())
      .then(data =>data.data);

      search_query.value="";

    }
    return{
      search_query,
      animeList,
      handleSearch

    }
  }
  
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Fira Sans", sans-serif;
}

a {
  text-decoration: none;
}

header {
  padding-top: 50px;
  padding-bottom: 50px;
}

header h1 {
  color: #888;
  font-size: 42px;
  font-weight: 400;
  text-align: center;
  text-transform: uppercase;
  margin-bottom: 30px;
}

h1 strong {
  color: #313131;
}

h1:hover {
  color: #313131;
}

.search-box{
  display: flex;
  padding-left: 30px;
  padding-right: 30px;
  justify-content: center;
}

.search-field{
  appearance: none;
  background: none;
  border: none;
  outline: none;
  background-color:#f3f3f3 ;
  box-shadow: 0px 4px 8px rgba(0,0,0,0.15);
  display: block;
  width: 100%;
  max-width: 600px;
  padding: 15px;
  border-radius: 8px;
  color: #313131;
  font-size: 20px;
  transition: 0.4s;


}

.search-field::placeholder{
  color: #aaa;

}

.search-field:focus,.search-field:valid{
  color: #fff;
  background-color: #313131;
  box-shadow: 0px 0px 0px rgba(0,0,0,0.15);

}

main{
  padding-left: 30px;
  padding-right: 30px;
  max-width: 1200px;
  margin: 0 auto;

}

.cards{
  display: flex;
  flex-wrap: wrap;
  margin: 0 -8px;
}

</style>
