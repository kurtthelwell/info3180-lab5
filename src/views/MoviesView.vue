<template>
  <div class="container mt-5">
    <h1 class="mb-4">Movies</h1>

    <div class="row row-cols-1 row-cols-md-2 g-4">
      <div class="col" v-for="movie in movies" :key="movie.id">
        <div class="card h-100">
          <div class="row g-0">
            <div class="col-4">
              <img :src="movie.poster" :alt="movie.title" class="img-fluid rounded-start poster-img" />
            </div>
            <div class="col-8">
              <div class="card-body">
                <h5 class="card-title">{{ movie.title }}</h5>
                <p class="card-text">{{ movie.description }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

let movies = ref([]);

function fetchMovies() {
  fetch("/api/v1/movies")
    .then((response) => response.json())
    .then((data) => {
      movies.value = data.movies;
    })
    .catch((error) => {
      console.log(error);
    });
}

onMounted(() => {
  fetchMovies();
});
</script>

<style scoped>
.poster-img {
  object-fit: cover;
  height: 100%;
  width: 100%;
}
</style>
