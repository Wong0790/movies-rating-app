<script setup>
import { StarIcon, PlayIcon } from "@heroicons/vue/24/outline";
import TrashSvg from "./TrashSvg.vue";
import EditSvg from "./EditSvg.vue";

defineProps(["movie", "index"]);
defineEmits(["edit", "remove"]);

const updateDescription = (description) => {
  return `${description.substring(0, 220)}...`;
};

const watchTrailer = (movie) => {
  window.open(movie.trailer, "_blank");
};
</script>

<template>
  <div class="movie-item">
    <div class="image-wrapper">
      <img :src="movie.image" :alt="`${movie.name} image`" class="movie-img" />
      <button
        class="options-btn left-options-btn"
        @click="$emit('remove', index)"
      >
        <TrashSvg />
      </button>
      <button
        class="options-btn right-options-btn"
        @click="$emit('edit', movie)"
      >
        <EditSvg />
      </button>
    </div>

    <div class="content-wrapper">
      <h6 class="movie-title">{{ movie.name }}</h6>
      <div class="sub-title-wrapper">
        <div class="genre-wrapper">
          <span v-for="genre in movie.genres" :key="genre" class="genre-concat">
            {{ genre }}
          </span>
        </div>
      </div>

      <p class="description-section">
        <span class="about-subtitle">About the Movie</span>
        {{
          movie.description.length > 220
            ? updateDescription(movie.description)
            : movie.description
        }}
      </p>

      <div class="btns-wrapper">
        <button class="buy-btn">Buy</button>
        <button class="trailer-btn btn-wrapper" @click="watchTrailer(movie)">
          <span>Watch trailer</span>
          <PlayIcon class="play-icon" />
        </button>
      </div>

      <div class="rating-wrapper">
        <span class="rating-text">({{ movie.rating }}/5)</span>
        <button @click="movie.rating = n" v-for="n in 5" :key="n">
          <StarIcon
            class="star-icon"
            :class="[
              movie.rating >= n ? 'gold-star-icon' : '',
              n === movie.rating ? 'cursor-not-allowed' : 'cursor-pointer',
            ]"
          />
        </button>
      </div>
    </div>
    <StarIcon class="big-star-rating gold-star-icon" />
    <div class="absolute-rating">{{ movie.rating }}</div>
  </div>
</template>
