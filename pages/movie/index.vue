<template>
  <h1>Movies</h1>
</template>

<script>
import axios from 'axios';
export default {
  data()
  {
    movies: []
  },
  mounted(){
    axios.get('http://localhost:3000/movie')
    .then((response) => console.log(response.data))
    .catch(err => console.log(err))
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
