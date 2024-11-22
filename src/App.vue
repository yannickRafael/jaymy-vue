<template>
  <div class="container">
    <header class="navbar">
      <h1 class="logo">jaymy Movies</h1>
      <input
        v-model="query"
        class="search-bar"
        type="text"
        placeholder="Search for a movie..."
        @input="fetchIMDBData"
      />
    </header>

    <main>
      <p v-if="isLoading" class="loading">Loading...</p>
      <p v-if="errorMessage" class="error">{{ errorMessage }}</p>

      <div v-if="movies.length" class="movie-list">
        <MovieCard
          v-for="movie in movies"
          :key="movie.imdbID"
          :movie="movie"
        />
      </div>
    </main>
  </div>
</template>

<script>
import { ref, watch, onMounted } from "vue";
import MovieCard from "./components/MovieCard.vue";

export default {
  name: "App",
  components: {
    MovieCard,
  },
  setup() {
    const query = ref("Avengers");
    const movies = ref([]);
    const isLoading = ref(false);
    const errorMessage = ref("");

    const apiKey = "6Kt1sfLrUqNhe4W94HcKMW:7IpNljkbED6o5jnriHRt1r";
    const url = (query) =>
      `https://api.collectapi.com/imdb/imdbSearchByName?query=${encodeURIComponent(query)}`;

    const fetchIMDBData = async () => {
      if (query.value.length < 3) return;

      isLoading.value = true;
      errorMessage.value = "";
      try {
        const response = await fetch(url(query.value), {
          method: "GET",
          headers: {
            authorization: `apikey ${apiKey}`,
            "content-type": "application/json",
          },
        });

        if (!response.ok) {
          throw new Error("Failed to fetch data.");
        }

        const data = await response.json();
        const movieData = data.result;

        if (Array.isArray(movieData) && movieData.length > 0) {
          movies.value = movieData;
        } else {
          movies.value = [];
          errorMessage.value = `No results found for "${query.value}".`;
        }
      } catch (error) {
        errorMessage.value = "Error fetching data. Please try again later.";
      } finally {
        isLoading.value = false;
      }
    };

    watch(query, fetchIMDBData);

    onMounted(() => {
      fetchIMDBData();
    });

    return {
      query,
      movies,
      isLoading,
      errorMessage,
    };
  },
};
</script>

<style>
.container {
  font-family: Arial, sans-serif;
  margin: 0 auto;
  padding: 20px;
}

.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #333;
  color: #fff;
  padding: 10px 20px;
}

.logo {
  font-size: 1.5rem;
}

.search-bar {
  padding: 5px 10px;
  font-size: 1rem;
}

.movie-list {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.loading,
.error {
  text-align: center;
  font-size: 1.2rem;
  margin-top: 20px;
}

.error {
  color: red;
}
</style>
