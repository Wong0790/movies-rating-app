<script setup>
import { ref, computed, onMounted } from "vue";
import { useVuelidate } from "@vuelidate/core";
import { required, url } from "@vuelidate/validators";

const props = defineProps(["movie"]);
const emit = defineEmits(["add"]);
const genres = [
  "action",
  "adventure",
  "animation",
  "biographical",
  "comedy",
  "crime",
  "disaster",
  "documentary",
  "drama",
  "horror",
  "fantasy",
  "fiction",
  "hindi",
  "martial-arts",
  "musical",
  "mystery",
  "narrative",
  "noir",
  "romance",
  "romantic-comedy",
  "science-fiction",
  "slasher",
  "sports",
  "thriller",
  "war",
  "western",
];

const nameInput = ref(null);
const movie = ref({
  id: undefined,
  name: "",
  description: "",
  image: "",
  rating: 0,
  genres: [],
  release: "",
  trailer: "",
});

const rules = computed(() => {
  return {
    name: { required },
    description: { required },
    image: { required, url },
    genres: { required },
    trailer: { required, url },
  };
});

const v$ = useVuelidate(rules, movie);

const submitForm = async () => {
  await v$.value.$validate();
  if (!v$.value.$error) {
    emit("add", movie.value);
  }
};

onMounted(() => {
  if (props.movie) {
    movie.value = props.movie;
  }
  nameInput.value.focus();
});
</script>

<template>
  <form>
    <label for="name">Name </label>
    <input
      type="text"
      id="name"
      v-model="movie.name"
      class="dialog-input"
      ref="nameInput"
      placeholder="Enter movie title"
      :class="v$.name.$error ? 'invalid-input' : ''"
    />
    <small v-if="v$.name.$error" class="invalid-text">{{
      v$.name.required.$message
    }}</small>
    <label for="description">Description </label>
    <textarea
      id="description"
      v-model="movie.description"
      class="dialog-input"
      rows="4"
      cols="50"
      placeholder="Enter description of the movie"
      :class="v$.description.$error ? 'invalid-input' : ''"
    ></textarea>
    <small v-if="v$.description.$error" class="invalid-text">{{
      v$.description.required.$message
    }}</small>
    <label for="image">Image </label>
    <input
      type="text"
      id="image"
      v-model="movie.image"
      class="dialog-input"
      placeholder="https://www.dreamworks.com/storage/cms-uploads/rgtk-poster.jpg"
      :class="v$.image.$error ? 'invalid-input' : ''"
    />
    <small
      v-if="v$.image.$error"
      v-for="error in v$.image.$errors"
      class="invalid-text"
      >{{ error.$message }}</small
    >
    <label for="genres"> Genre </label>
    <select
      id="genres"
      name="genres"
      class="genres-select"
      multiple
      v-model="movie.genres"
      :class="v$.genres.$error ? 'invalid-input' : ''"
    >
      <option v-for="genre in genres" :value="genre">
        {{ genre }}
      </option>
    </select>
    <small v-if="v$.genres.$error" class="invalid-text">{{
      v$.genres.required.$message
    }}</small>
    <label for="trailer">Trailer </label>
    <input
      type="text"
      id="trailer"
      v-model="movie.trailer"
      class="dialog-input"
      placeholder="https://www.youtube.com/watch?v=f_fuHRyQbOc"
      :class="v$.trailer.$error ? 'invalid-input' : ''"
    />
    <small
      v-if="v$.trailer.$error"
      v-for="error in v$.trailer.$errors"
      class="invalid-text"
      >{{ error.$message }}</small
    >
    <button class="submit-btn" @click.prevent.stop="submitForm">
      <slot />
    </button>
  </form>
</template>
