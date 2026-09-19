<script setup lang="ts">
import ModalDemo from "./components/ModalDemo.vue";
</script>

<template>
  <main class="page">
    <h1>Accessible modal</h1>
    <p>Escape, focus trap, and focus restore.</p>
    <ModalDemo />
  </main>
</template>

<style scoped>
.page {
  max-width: 40rem;
  margin: 2rem auto;
  padding: 0 1rem;
}
</style>
