<script setup>
import { RouterView } from "vue-router";
</script>

<template>
  <div class="flex gap-4">
    <RouterLink to="/">Home</RouterLink>
    <RouterLink to="/about">About</RouterLink>
    <RouterLink to="/gsap">Gsap</RouterLink>
  </div>
  <RouterView />
</template>

<style scoped></style>
