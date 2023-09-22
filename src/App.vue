<script setup>
import { items } from "./movies.json";
import { ref, computed, defineAsyncComponent } from "vue";
import MovieItem from "./components/MovieItem.vue";
import MovieForm from "./components/MovieForm.vue";
const AppModal = defineAsyncComponent(() =>
  import("./components/AppModal.vue")
);

const moviesList = ref(items);
const addMovieModal = ref(false);
let tempMovie = ref(null);
const average = computed(() => {
  let sum = 0;
  moviesList.value.forEach((movie) => {
    sum += movie.rating;
  });
  return (sum / moviesList.value.length).toFixed(0);
});

const addMovie = (newMovie) => {
  addMovieModal.value = false;
  if (newMovie.id) {
    const index = moviesList.value.findIndex((x) => x.id === newMovie.id);
    moviesList.value[index] = newMovie;
    tempMovie.value = null;
  } else {
    newMovie.id = moviesList.value.length + 1;
    moviesList.value.push(newMovie);
  }
};

const editMovie = (item) => {
  tempMovie.value = Object.assign(item);
  addMovieModal.value = true;
};
</script>

<template>
  <div
    class="app"
    :style="addMovieModal ? 'overflow-y: hidden' : 'overflow-y: auto'"
  >
    <div class="header">
      <div>
        <h1 class="total-movies">Total movies: {{ moviesList.length }}</h1>
        <h1 class="total-movies">Average rating: {{ average }}</h1>
      </div>
      <button class="add-movie-btn" @click="addMovieModal = true">
        Add movie
      </button>
    </div>
    <div class="movies-container">
      <MovieItem
        v-for="(movie, index) in moviesList"
        :key="movie.id"
        :movie="movie"
        :index="index"
        @edit="editMovie"
        @remove="moviesList.splice(index, 1)"
      />
    </div>
    <AppModal
      v-if="addMovieModal"
      :title="tempMovie ? 'Update movie' : 'Add movie'"
      @close="
        addMovieModal = false;
        tempMovie = null;
      "
    >
      <template #body>
        <MovieForm :movie="tempMovie" @add="addMovie">
          {{ tempMovie ? "Update" : "Add" }}
        </MovieForm>
      </template>
    </AppModal>
  </div>
</template>
