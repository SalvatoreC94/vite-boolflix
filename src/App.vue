<script>
/* 
  Per importare ed utilizzare un componente dentro un altro devo SEMPRE seguire questi 3 passi:
  1) Importazione del componente
  2) Dichiarazione del componente
  3) Utilizzo del componente
*/
// 1) Importazione del componente
import axios from 'axios'
 
export default {
  data() {
    return { 
      apiKey:'e5b9f07cb974b5607241361f444c4724',
      searchtext: '',
      movies:[]
    }
  },
  // 2) Dichiarazione del componente
  components: {
    
  },
  methods: {
  search(){
    console.log(this.searchtext);

    axios
    // .get('https://api.themoviedb.org/3/search/movie?api_key=' + this.apikey +'&query' + this.searchtext)
    .get('https://api.themoviedb.org/3/search/movie',{
      params:{
       api_Key: this.apiKey,
       query:this.searchttext,
      }
    })
    .then((resp) => {
    console.log(resp.data)

    this.movies = resp.data.results
    });
  }
  }
}
</script>

<template>
  
    <!-- 3) Utilizzo del componente -->
    <div class="d-flex justify-content-between">
<form @submit.prevent="search">
  <input v-model="searchtext" type="text" placeholder="cerca film e serie">
  <button type="submit">
    cerca
  </button>
</form>
    </div>
    

    <div>
    <ol>
      <li v-for="(movie, i) in movies" .Key="i">
      <ul>
        <li>
          titolo:
        </li>
        <li>
          titolo originale:
        </li>
        <li>
          lingua:
        </li>
        <li>
          voto:
        </li>
      </ul>
      <hr>
      </li>
    </ol>
    </div>
    
      
 
</template>

<style lang="scss">
@use 'assets/scss/main' as *;
// Import all of Bootstrap's CSS
@import "bootstrap/scss/bootstrap";
</style>
