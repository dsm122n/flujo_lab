<script setup>
import { ref, onMounted, computed } from 'vue';
import { labResults  } from "../stores/labResultsStore.js";
import { LabResultClass } from "../models/LabResultsClass.js";

const datitosLab = computed(() => labResults.value);

console.log(datitosLab);
console.log("Hola estos son los datitos lab");
console.log(datitosLab.length);
const highlightedRow = ref(null);
const highlightedColumn = ref(null);

const highlightRow = (rowIndex) => {
    highlightedRow.value = rowIndex;
};

const highlightColumn = (colIndex) => {
    highlightedColumn.value = colIndex;
};

const highlightCell = (rowIndex, colIndex) => {
    highlightedRow.value = rowIndex;
    highlightedColumn.value = colIndex;
};

const clearHighlight = () => {
    highlightedRow.value = null;
    highlightedColumn.value = null;
};

// onMounted(() => {
//     fetch("http://localhost:3000/laboratorios")
//         .then((response) => response.json())
//         .then((data) => {
//             datitosLab.value = data;
//         })
//         .catch((error) => {
//             console.error(error);
//         });
// });
</script>

<template>
    <h1>Flujo de Exámenes</h1>

    <div v-if="datitosLab.length > 0">
        <div class="table-container">
            <table class="vertical" id="table-for-print">
                <thead>
                    <tr>
                        <th v-for="key in Object.keys(datitosLab[0].extractedData)" :key="key"> {{ key }}</th>
                    </tr>
                </thead>
                <tbody>
                    <tr 
                        v-for="(item, rowIndex) in datitosLab" 
                        :key="rowIndex"
                        @mouseover="highlightRow(rowIndex)"
                        @mouseleave="clearHighlight"
                        :class="{ 'highlight-row': highlightedRow === rowIndex }">
                        <td 
                            v-for="(value, colIndex) in Object.values(item.extractedData)" 
                            :key="colIndex"
                            @mouseover="highlightCell(rowIndex, colIndex)"
                            @mouseleave="clearHighlight"
                            :class="{ 
                                'highlight-row': highlightedRow === rowIndex,
                                'highlight-col': highlightedColumn === colIndex
                            }">
                            {{ value }}
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>

    <div v-else>
        <p>No hay resultados de laboratorio disponibles.</p>
    </div>
</template>


<style scoped>
/* Everyone Else's Grid */
@supports (display: grid) {
  .vertical {
    display: grid;
    grid-template-columns: max-content;
    grid-template-areas: "head body";
  }
  .vertical thead {
    grid-area: head;
  }
  .vertical tbody {
    grid-area: body;
  }
}

/* Flex - Cross Browser CSS */
.vertical thead {
  display: flex;
  flex-shrink: 0;
  min-width: max-content;
}
.vertical tbody {
  display: flex;
  min-width: max-content;

}
.vertical tr {
  display: flex;
  flex-direction: column;
  min-width: min-content;
  flex-shrink: 0;
}
.vertical td, .vertical th {
  display: block;
  padding: 0 1em 0 1em;
    min-width: max-content;
}

.table-container {
  overflow-x: auto;
  width: 100%;
}

table { 
    border-collapse: collapse; 
}
table td {
  border: 1px solid black; 
  background-color: #ffffff00;
}
table th {
    text-wrap: false;
    
  border: 1px solid black;
  background-color: #ffc800;
  color: rgb(0, 0, 0);
  text-align: left;
}
/* change background in groups of 5 */
table tr:nth-child(10n), tr:nth-child(10n-1), tr:nth-child(10n-2), tr:nth-child(10n-3), tr:nth-child(10n-4) {
  background-color: #d2d2d2;
  color: #000;
}
/* on hover, change background color of columns and rows */
.highlight-row {
  background-color: #ffa2e577; /* Light pink */
}

.highlight-col {
  background-color: #00ffbb33; /* Light purple */
}

@media (preffers-color-scheme: light) {
  .highlight-row {
    background-color: #ff77d877; /* Light pink */
  }

  .highlight-col {
    background-color: #1ff0b833; /* Light purple */
  }
}

@media print {
    table {
        page-break-inside: auto;
        font-size: 10pt;
        background-color: #000000;
    }

}
</style>
