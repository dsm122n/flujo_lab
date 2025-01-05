<script>
export default {
    data() {
        return {
            fileNames: [], // Array of file names
        };
    },
    methods: {
        handleFileChange(event) {
            const files = event.target.files;
            this.fileNames = [];
            for (let i = 0; i < files.length; i++) {
                this.fileNames.push(files[i].name);
            }
        },
        moveUp(index) {
            if (index > 0) {
                const temp = this.fileNames[index];
                this.fileNames.splice(index, 1); // Remove the current item
                this.fileNames.splice(index - 1, 0, temp); // Insert it one position up
            }
        },
        moveDown(index) {
            if (index < this.fileNames.length - 1) {
                const temp = this.fileNames[index];
                this.fileNames.splice(index, 1); // Remove the current item
                this.fileNames.splice(index + 1, 0, temp); // Insert it one position down
            }
        },
    },
};
</script>

<template>
    <h1>Ingreso de lab new</h1>
    <div class="container">
      <div class="container-ingresos">
          <label for="link">Ingrese link aquí: </label>
          <input type="text" id="link" name="link">
          <button>Añadir link a flujo de exámenes</button>
      </div>
      <div class="container-ingresos">
          <label for="archivo">O suba los archivos aquí: </label>
          <input type="file" id="archivo" name="archivo" accept="application/pdf" multiple ref="input" @change="handleFileChange" />
          <p>Nombres de archivos: </p>
          <ol>
              <li v-for="(file, index) in fileNames" :key="index">
                  {{ file }}
                  <!-- Buttons to move the file up or down -->
                  <button @click="moveUp(index)" :disabled="index === 0">↑</button>
                  <button @click="moveDown(index)" :disabled="index === fileNames.length - 1">↓</button>
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
  