<script setup>
import { ref } from 'vue';
import * as pdfjsLib from "pdfjs-dist/webpack";

pdfjsLib.GlobalWorkerOptions.workerSrc = require("pdfjs-dist/build/pdf.worker.js");


const fileNames = ref([]);

// Handle file change
const handleFileChange = (event) => {
  const files = event.target.files;
  fileNames.value = []; // Clear the array before adding new files
  for (let i = 0; i < files.length; i++) {
    fileNames.value.push(files[i].name);
  }
};

// Move file up in the list
const moveUp = (index) => {
  if (index > 0) {
    const temp = fileNames.value[index];
    fileNames.value.splice(index, 1); // Remove the current item
    fileNames.value.splice(index - 1, 0, temp); // Insert it one position up
  }
};

// Move file down in the list
const moveDown = (index) => {
  if (index < fileNames.value.length - 1) {
    const temp = fileNames.value[index];
    fileNames.value.splice(index, 1); // Remove the current item
    fileNames.value.splice(index + 1, 0, temp); // Insert it one position down
  }
};
</script>

<template>
    <h1>Ingreso de lab new</h1>
    <div class="container">
      <div class="container-ingresos">
          <label for="link">Ingrese link aquí: </label>
          <input type="text" id="link" name="link">
          <button @click="importPDF" >Añadir link a flujo de exámenes</button>
      </div>
      <div class="container-ingresos">
          <label for="archivo">O suba los archivos aquí: </label>
          <input type="file" id="archivo" name="archivo" accept="application/pdf" multiple @change="handleFileChange" />
          <p>Nombres de archivos: </p>
          <ol>
              <li v-for="(file, index) in fileNames" :key="index">
                  <button @click="moveUp(index)" :disabled="index === 0">↑</button>
                  <button @click="moveDown(index)" :disabled="index === fileNames.length - 1">↓</button>
                  {{ file }}
              </li>
          </ol>
          <button>Añadir archivos a flujo de exámenes</button>
      </div>
    </div>
</template>


<style scoped>
.container {
  display: flex;
  flex-direction: row;
  align-items: top;
}
.container-ingresos {
    display: flex;
    flex-direction: column;
    align-items: left;
    margin: 1em;
    border: 1px solid black;
    border-radius: 1em;
    background-color: #75e8ff39;
    padding: 1em;
}
input {
    margin: 0.5em;
    text-align: left;
    vertical-align: top;
}
button {
    margin-left: 0.5em;
    padding: 0.2em 0.5em;
    cursor: pointer;
}
button:disabled {
    background-color: #ddd;
    cursor: not-allowed;
}
</style>
