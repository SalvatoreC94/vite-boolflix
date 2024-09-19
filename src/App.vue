<script>
import axios from 'axios'

export default {
  data() {
    return {
      apiKey: 'e5b9f07cb974b5607241361f444c4724',
      searchtext: '',
      searchType: 'movie', // Valore di default per cercare film
      movies: [],
      series: []
    }
  },
  methods: {
    searchContent() {
      const url =
        this.searchType === 'movie'
          ? 'https://api.themoviedb.org/3/search/movie'
          : 'https://api.themoviedb.org/3/search/tv';

      axios
        .get(url, {
          params: {
            api_key: this.apiKey,
            query: this.searchtext
          }
        })
        .then(res => {
          if (this.searchType === 'movie') {
            this.movies = res.data.results;
            this.series = []; // Reset delle serie quando si cercano i film
          } else {
            this.series = res.data.results;
            this.movies = []; // Reset dei film quando si cercano le serie
          }
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
    <form @submit.prevent="searchContent" class="d-flex mb-4">
      <input
        v-model="searchtext"
        type="text"
        class="form-control me-2"
        placeholder="Cerca film o serie TV"/>
      <select v-model="searchType" class="form-select me-2">
        <option value="movie">Film</option>
        <option value="tv">Serie TV</option>
      </select>
      <button type="submit" class="btn btn-primary">Cerca</button>
    </form>

    <div class="list-group">
      <div
        v-for="(movie, index) in movies" :key="index"
        class="list-group-item list-group-item-action">
        <h5 class="mb-1">{{ movie.title }}</h5>
        <p class="mb-1"> 
          <strong>Titolo Originale:</strong> {{ movie.original_title }}
        </p>
        <p class="mb-1">
          <template v-if="movie.original_language === 'en'">
            <strong>Lingua:</strong> <img src="/img/flags/en.jpg" alt="">
          </template>
          <template v-else-if="movie.original_language === 'it'">
            <strong>Lingua:</strong> <img src="/img/flags/it.jpg" alt="">
          </template>
          <template v-else-if="movie.original_language === 'ja'">
            <strong>Lingua:</strong> <img src="/img/flags/jp.png" alt="">
          </template>
          <template v-else>
            <strong>Lingua:</strong> {{ movie.original_language }}
          </template>
        </p>
        <small><strong>Voto:</strong> {{ movie.vote_average }}</small>
      </div>

      <div
        v-for="(serie, index) in series" :key="index"
        class="list-group-item list-group-item-action">
        <h5 class="mb-1">{{ serie.name }}</h5>
        <p class="mb-1">
          <strong>Titolo Originale:</strong> {{ serie.original_name }}
        </p>
        <p class="mb-1">
          <template v-if="serie.original_language === 'en'">
            <strong>Lingua:</strong> <img src="/img/flags/en.jpg" alt="">
          </template>
          <template v-else-if="serie.original_language === 'it'">
            <strong>Lingua:</strong> <img src="/img/flags/it.jpg" alt="">
          </template>
          <template v-else-if="serie.original_language === 'ja'">
            <strong>Lingua:</strong> <img src="/img/flags/jp.png" alt="">
          </template>
          <template v-else>
            <strong>Lingua:</strong> {{ serie.original_language }}
          </template>
        </p>
        <small><strong>Voto:</strong> {{ serie.vote_average }}</small>
      </div>
    </div>
  </div>
</template>

<style lang="scss">
@use 'assets/scss/main' as *;
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

img {
  max-width: 40px;
}
</style>
