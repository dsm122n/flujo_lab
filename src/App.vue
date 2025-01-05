<script setup>
import HelloWorld from './components/HelloWorld.vue'
import IngresoLab from './components/IngresoLab.vue'
import VisorFlujo from './components/VisorFlujo.vue'
</script>

<template>

  <IngresoLab />
  <VisorFlujo />

</template>

<style scoped>
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
