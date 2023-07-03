<script setup lang="ts">
import HelloWorld from './components/HelloWorld.vue';
import test2 from './components/test2.vue';
</script>

<template>
  <div class="main">
    <h1>d3.js test</h1>
    <HelloWorld msg="Vite + Vue" />
    <test2></test2>
  </div>
</template>

<style scoped>
.main {
  width: 100%;
  height: 100%;
}
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
