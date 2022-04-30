<template>
  <h1>Movies</h1>
</template>

<script>
import axios from 'axios';
// import vue from 'vue';

export default {
  name: "ThisMovies",
  // data: () => ({
  //   movies:[]
  // }),
  data() {
    return{
      movies:[]
    }
  },
  async mounted(){
    const response = await axios.get('http://localhost:3000/movies')
    this.movies = response.data
    // .then((response) => response.data,this.movies)
    // .catch(err => console.log(err))

    // .catch(err => console.log(err)) 
    // fetch('http://localhost:3000/movie')
    // .then(res => res.json)
    // .then(data => this.movie = data)
    // .catch(err => console.log(err.massage))
  }
}
</script>

<style>
</style>
