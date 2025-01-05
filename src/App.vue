<script setup>
import HelloWorld from './components/HelloWorld.vue'
import FlujoNew from './components/FlujoNew.vue'
import IngresoLab from './components/IngresoLab.vue'
import IngresoLabNew from './components/IngresoLabNew.vue'
</script>

<template>
  <IngresoLabNew />
  <FlujoNew />
  
</template>

<style>
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
