<script setup>
import { ref } from 'vue';
import * as pdfjsLib from "pdfjs-dist";
import "pdfjs-dist/build/pdf.worker";
import { labResults } from "../stores/labResultsStore.js";
import { LabResultClass } from "../models/LabResultsClass.js";
//const labResults = ref([]); // Almacena los resultados en formato JSON

const nombreExamenes = [
    "Creatinina","Lactato","Sodio (Na)","Potasio (K)","Cloro (Cl)","proBNP", "Calcio","Uremia","Nitrogeno ureico","Magnesio","Lactato", "Fosforo",
    "Bilirrubina Total","Bilirrubina Directa","ASAT/GOT","ALAT/GPT","Gama GT","Fosfatasas Alcalinas",
    "Troponina I", "LDH", "Albumina",
    "Proteinas", "PCR", "Glucosa","Trigliceridos", "CK Total", "CK MB", "V.H.S.",
   
    "Hematocrito", "Hemoglobina", "Recuento Hematíes", "VCM", "HCM", "CHCM", 
    "Recuento Leucocitos", "Recuento de Plaquetas", "RDW", 
    "Segmentados %", "Linfocitos %", "Monocitos %", "Basófilos %", 
    "Eosinófilos %", "Eosinófilos", "Basófilos", "Segmentados", 
    "Linfocitos", "Monocitos",

    "pH","pO2","pCO2","tCO2","HCO3-","BE (B)","sO2",

    "INR","Tiempo de protrombina %","Tiempo de protrombina seg","TTPK"
]

const loadPdf = async (file) => {
    try {
        const fileReader = new FileReader();
        fileReader.onload = async (e) => {
            const pdfData = e.target.result;
            const pdf = await pdfjsLib.getDocument({ data: pdfData }).promise;
            let extractedData = new LabResultClass();

            

            for (let i = 1; i <= pdf.numPages; i++) {

                const page = await pdf.getPage(i);
                const textContent = await page.getTextContent();
                const pageText = textContent.items.map((item) => item.str).join(" ");
                
                if (i === 1) {
                    const regex = /Fecha y Hora Ingreso Solicitud\s*:\s*(\d{2}-\d{2}-\d{4})\s+(\d{2}:\d{2}:\d{2})/i;

                    const match = pageText.match(regex);

                    if (match) {
                        const [_, datePart, timePart] = match; // Extract the matched groups
                        console.log("Date:", datePart);
                        console.log("Time:", timePart);

                        // Convert to JavaScript Date object
                        const [day, month, year] = datePart.split("-");
                        const [hours, minutes, seconds] = timePart.split(":");
                        const dateObject = new Date(year, month - 1, day, hours, minutes, seconds);
                        extractedData.fecha = dateObject;

                        console.log("JavaScript Date Object:", dateObject);
                    }
                }

                // Procesa el texto de cada página para extraer los datos relevantes
                console.log(`Texto de la página ${i}:`, pageText);
                for (let examen of nombreExamenes) {

                    const regex = new RegExp(`${examen}\\s*[:]?\\s*(\\d+[.,]?\\d*\\s*\\*?)`, "i");
                    const match = pageText.match(regex);

                    if (match) {
                        // Store the full matched value as a string, including * if present
                        extractedData[examen] = match[1]; 
                        // delete blank spaces
                        extractedData[examen] = extractedData[examen].replace(/\s/g, '');

                    }
                }
                
            }
            console.log("Datos extraídos:", extractedData);
            // Agregar los datos extraídos al array

            labResults.value.push({
                fileName: file.name,
                extractedData,
            });
            console.log("Resultados de laboratorio:", labResults.value);
        };

        fileReader.readAsArrayBuffer(file);
        console.log("Hola estos son los datitos lab desde el ingreso lab new");
        
    } catch (error) {
        console.error("Error al cargar el PDF:", error);
    }
};




const importPDFArchivo = () => {
    const fileInput = document.getElementById("archivo");
    if (fileInput.files.length === 0) {
        console.error("No se han seleccionado archivos");
        return;
    }

    for (let i = 0; i < fileInput.files.length; i++) {
        loadPdf(fileInput.files[i]);
    }
};


const fileNames = ref([]);

// Manejar cambios en los archivos
const handleFileChange = (event) => {
    const files = event.target.files;
    fileNames.value = []; // Limpiar el array antes de agregar nuevos archivos
    for (let i = 0; i < files.length; i++) {
        fileNames.value.push(files[i].name);
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
                   
                    {{ file }}
                </li>
            </ol>
            <button @click="importPDFArchivo" >Añadir archivos a flujo de exámenes</button>
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
