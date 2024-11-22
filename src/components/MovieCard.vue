<template>
    <div class="movie-card">
      <a :href="movie.Poster" target="_blank">
        <img :src="movie.Poster" :alt="movie.Title" />
      </a>
      <h2>{{ movie.Title }}</h2>
      <p>{{ movie.Year }}</p>
    </div>
  </template>
  
  <script>
  export default {
    name: "MovieCard",
    props: {
      movie: Object,
    },
  };
  </script>
  
  <style scoped>
  .movie-card {
    width: 200px;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
    text-align: center;
  }
  
  .movie-card img {
    width: 100%;
    height: auto;
    margin-bottom: 10px;
  }
  
  .movie-card h2 {
    font-size: 1.1rem;
    margin: 5px 0;
  }
  
  .movie-card p {
    font-size: 0.9rem;
    color: #555;
  }
  </style>
  