<script setup>
import CloseSvg from "./CloseSvg.vue";

const emit = defineEmits(["close"]);
const props = defineProps(["title"]);
</script>

<template>
  <div class="outside-wrapper"></div>
  <div class="dialog-form animate-fade-in">
    <div class="dialog-header">
      <h1 class="dialog-title">{{ title }}</h1>
      <button class="close-btn" @click="$emit('close')">
        <CloseSvg />
      </button>
    </div>
    <div class="dialog-content">
      <slot name="body"></slot>
    </div>
  </div>
</template>
