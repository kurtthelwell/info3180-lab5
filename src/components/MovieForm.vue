<template>
  <form id="movieForm" @submit.prevent="saveMovie">

    <div v-if="successMessage" class="alert alert-success">
      {{ successMessage }}
    </div>

    <div v-if="errors.length > 0" class="alert alert-danger">
      <ul class="mb-0">
        <li v-for="error in errors" :key="error">{{ error }}</li>
      </ul>
    </div>

    <div class="form-group mb-3">
      <label for="title" class="form-label">Movie Title</label>
      <input type="text" name="title" id="title" class="form-control" />
    </div>

    <div class="form-group mb-3">
      <label for="description" class="form-label">Description</label>
      <textarea name="description" id="description" class="form-control" rows="4"></textarea>
    </div>

    <div class="form-group mb-3">
      <label for="poster" class="form-label">Movie Poster</label>
      <input type="file" name="poster" id="poster" class="form-control" accept=".jpg,.jpeg,.png,.webp" />
    </div>

    <button type="submit" class="btn btn-primary">Add Movie</button>
  </form>
</template>

<script setup>
import { ref, onMounted } from "vue";

let csrf_token = ref("");
let successMessage = ref("");
let errors = ref([]);

function getCsrfToken() {
  fetch('/api/v1/csrf-token')
    .then((response) => response.json())
    .then((data) => {
      csrf_token.value = data.csrf_token;
    })
    .catch((error) => {
      console.log(error);
    });
}

function saveMovie() {
  let movieForm = document.getElementById('movieForm');
  let form_data = new FormData(movieForm);

  successMessage.value = "";
  errors.value = [];

  form_data.append('csrf_token', csrf_token.value);

  fetch("/api/v1/movies", {
    method: 'POST',
    body: form_data,
    headers: {
      'X-CSRFToken': csrf_token.value
    }
  })
    .then(function (response) {
      return response.json();
    })
    .then(function (data) {
      if (data.errors) {
        errors.value = data.errors;
      } else {
        successMessage.value = data.message;
        movieForm.reset();
      }
    })
    .catch(function (error) {
      console.log(error);
    });
}

onMounted(() => {
  getCsrfToken();
});
</script>

<style scoped>
/* Add any component specific styles here */
</style>
