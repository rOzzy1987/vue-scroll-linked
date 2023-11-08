<script setup lang="ts">
import HomeView from './views/HomeView.vue';
</script>

<template>
  <HomeView />
  <footer>
    Images by <a href="https://www.freepik.com/">Freepik</a>
  </footer>
</template>

<style scoped>
footer {
  position: fixed;
  bottom: 0;
  background: rgba(255,255,255,.5);
  padding: 8px 40px;
}

</style>
