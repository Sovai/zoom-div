<script setup>
import { RouterView } from "vue-router";
</script>

<template>
  <div class="flex gap-4">
    <RouterLink to="/">Home</RouterLink>
    <RouterLink to="/about">About</RouterLink>
  </div>
  <RouterView />
</template>

<style scoped></style>
