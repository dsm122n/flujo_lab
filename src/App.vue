<script setup>
import HelloWorld from './components/HelloWorld.vue'
import FlujoNew from './components/FlujoNew.vue'
import IngresoLab from './components/IngresoLab.vue'
import IngresoLabNew from './components/IngresoLabNew.vue'

// print content on FlujoNew
const print = () => {
  const printContent = document.getElementById('table-for-print').outerHTML;
  const originalContent = document.body.innerHTML;
  document.body.innerHTML = `<table>${printContent}</table>`;
  window.print();
  document.body.innerHTML = originalContent;
};

</script>

<template>
  <IngresoLabNew />
  <button @click="print" id="print-button">Imprimir flujo de exámenes</button>
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
