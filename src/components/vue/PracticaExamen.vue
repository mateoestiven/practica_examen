<script setup>

import { ref } from 'vue';

const selectedIndexPadre = ref(-1);
const selectedIndexHijo = ref(-1);
const selectedIndexNieto = ref(-1);


const files = ref([
    { name: 'Home', icon: '🏠' },
    { name: 'Galery', icon: '🖼' },
    {
        name: 'OneDrive-Personal', icon: '📁', hijo: [
            
            {
                name: 'Documents', icon: '📁', hijo: [
                    { name: 'Anvsoft', icon: '📁' },
                    { name: 'Capture', icon: '📁' },
                    { name: 'ClickCharts', icon: '📁' },
                    { name: 'Custom Office', icon: '📁' },
                    { name: 'Onenote', icon: '📁' },
                    { name: 'Sound Recordings', icon: '📁' },
                    { name: 'Zoom', icon: '📁' }
                ]
            },
            { name: 'Pictures', icon: '📁' },
            { name: 'TWC', icon: '📁' }
        ]
    }
]);

function showInfoPadre(i) {
    if (i == selectedIndexPadre.value) {
        selectedIndexPadre.value = -1
    } else {
        selectedIndexPadre.value = i
    }
}

function showInfoHijo(j) {
    if (j == selectedIndexHijo.value) {
        selectedIndexHijo.value = -1
    } else {
        selectedIndexHijo.value = j
    }
}

function showInfoNieto(z) {
    if (z == selectedIndexNieto.value) {
        selectedIndexNieto.value = -1
    } else {
        selectedIndexNieto.value = z
    }
}

function addFile(){
    if (selectedIndexPadre.value !== -1) {
        const newFile = {name:'Nuevo Archivo', icon:'📄'}
        if(files.value[selectedIndexPadre.value].hijo){
            files.value[selectedIndexPadre.value].hijo.push(newFile)
        }else{
            files.value[selectedIndexPadre.value].hijo = [newFile]
        }
    }else{
        alert('No se encontro ruta')
    }
}


</script>

<template>
    <div class="h-screen w-full flex justify-center items-center bg-purple-400">
        <div class="w-2/4 flex  ">
            <div class="w-2/4 bg-slate-50 select-none h-[300px] overflow-y-auto">
                <div v-for="(file, i) in files" :key="i" class="font-medium">
                    <div @click="showInfoPadre(i)" class=" w-1/2 cursor-pointer">
                        <span>{{ selectedIndexPadre == i ? 'v' : '>' }}</span>
                        {{  file.icon }}
                        {{  file.name }}
                    </div>
                    <div v-show="selectedIndexPadre == i" class="px-2">
                        <div  v-for="(hijo, j) in file.hijo" :key="j" class="">
                            <div @click="showInfoHijo(j)" class=" w-1/2 cursor-pointer">
                                <span>{{ selectedIndexHijo == j ? 'v' : '>' }}</span>
                                {{ hijo.icon }}
                                {{ hijo.name }}
                            </div>
                            <div v-show="selectedIndexHijo==j" class="">
                            <div v-for="(nieto, z) in hijo.hijo" :key="z" class="px-2">
                                <div @click="showInfoNieto(z)" class="w-1/2 cursor-pointer">
                                    <span>{{ selectedIndexNieto == z ? 'v' : '>' }}</span>
                                    {{ nieto.icon }}
                                    {{ nieto.name }}
                                </div>
                            </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="w-2/4 bg-slate-600 text-center grid grid-rows-2 justify-center items-center ">
                <div class="bg-green-400 px-2 py-2 w-32 h-10 rounded-2xl cursor-pointer font-semibold mt-24">
                    añadir carpeta
                </div>
                <div @click="addFile()" class="bg-blue-400 px-2 py-2 w-32 h-10 rounded-2xl cursor-pointer font-semibold mb-24">
                    añadir archivo
                </div>
            </div>
        </div>
    </div>
</template>