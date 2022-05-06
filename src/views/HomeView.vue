<script>import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      movies: [],
      newMovieParams: {},
      errors: []
    };
  },
  created: function () {
    console.log("in create...")
  },
  methods: {
    moviesIndex: function () {
      console.log('In movies...');
      axios.get(`http://localhost:3000/movies.json`).then(response => {
        console.log(response.data);
        this.movies = response.data;
      });
    },
    createMovie: function () {
      console.log('In creating a new movie...');
      var newMovieParams = {
        title: this.newMovieParams.title,
        year: this.newMovieParams.year,
        plot: this.newMovieParams.plot,
        director: this.newMovieParams.director,
        image_url: this.newMovieParams.image_url
      }
      axios.post(`http://localhost:3000/movies.json`, newMovieParams).then(response => {
        console.log(response.data);
        this.movies.push(response.data);
        this.errors = [];
        this.newMovieParams = {}
      }).catch(error => {
        console.log('something bad happened');
        console.log(this.errors = error.response.data.errors);
      })
    }
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <!-- <p>{{ movies }}</p> -->
    <small v-for="error in errors">
      {{ error }}
      <br />
    </small>
    <button v-on:click="moviesIndex()">Show me allllll the movies</button>

    <br />
    <div v-for="movie in movies" v-bind:key="movie.id">
      <br />
      {{ movie.id }}. {{ movie.title }}
      <br />
      <img v-bind:src="movie.image_url" v-bind:alt="movie.title" />
      <p>Year: {{ movie.year }}</p>
      <p>Plot: {{ movie.plot }}</p>
      <p>Directed by: {{ movie.director }}</p>
      <p>Genres: {{ movie.genres }}</p>
      <br />
    </div>
    <br />
    <p>
      Title:
      <input v-model="newMovieParams.title" />
    </p>
    <p>
      Year:
      <input v-model="newMovieParams.year" />
    </p>
    <p>
      Plot:
      <input v-model="newMovieParams.plot" />
    </p>
    <p>
      Director:
      <input v-model="newMovieParams.director" />
    </p>
    <p>
      Image URL:
      <input v-model="newMovieParams.image_url" />
    </p>

    <button v-on:click="createMovie()">Add a new movie!</button>
  </div>
</template>

<style>
img {
  width: 300px;
}
</style>