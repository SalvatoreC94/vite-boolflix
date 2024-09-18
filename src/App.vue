<script>
import axios from 'axios'

export default {
  data() {
    return {
      apiKey: 'e5b9f07cb974b5607241361f444c4724',
      searchtext: '',
      movies: []
    }
  },
  methods: {
    searchMovies() {
      axios
        .get('https://api.themoviedb.org/3/search/movie', {
          params: {
            api_key: this.apiKey,
            query: this.searchtext
          }
        })
        
        .then(response => {
          this.movies = response.data.results;
        })
        .catch(error => {
          console.error("Errore nell'API:", error);
        });
    }
  }
}
</script>

<template>
<div class="container mt-5">
    <form @submit.prevent="searchMovies" class="d-flex mb-4">
      <input
        v-model="searchtext"
        type="text"
        class="form-control me-2"
        placeholder="Cerca film o serie"
      />
      <button type="submit" class="btn btn-primary">Cerca</button>
    </form>

    <!-- Movies List -->
    <div v-if="movies.length > 0">
      <h3 class="mb-3">Risultati:</h3>
      <div class="list-group">
        <a
          v-for="(movie, index) in movies"
          :key="index"
          class="list-group-item list-group-item-action">
          <h5 class="mb-1">{{ movie.title || movie.name }}</h5>
          <p class="mb-1">
            <strong>Titolo Originale:</strong> {{ movie.original_title || movie.original_name }}
          </p>
          <p class="mb-1">
            <strong>Lingua:</strong> {{ movie.original_language }}
          </p>
          <small><strong>Voto:</strong> {{ movie.vote_average }}</small>
        </a>
      </div>
    </div>
</div>
    
</template>



<style lang="scss">
@use 'assets/scss/main' as * ;
/*  Import all of Bootstrap's CSS */
@import "bootstrap/scss/bootstrap";


.container {
  max-width: 800px;
  margin: auto;
}

.list-group-item {
  border: 1px solid #ddd;
  margin-bottom: 10px;
}

</style>
