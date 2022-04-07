<script>import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      movies: [],
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
    }
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <!-- <p>{{ movies }}</p> -->

    <button v-on:click="moviesIndex()">Show me allllll the movies</button>

    <br />
    <div v-for="movie in movies" v-bind:key="movie.id">
      <br />
      {{ movie.id }}. {{ movie.title }}
      <p>Plot: {{ movie.plot }}</p>
    </div>
  </div>
</template>

<style></style>