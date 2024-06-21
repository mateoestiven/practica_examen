<script setup>
import { ref } from 'vue';

const rangosDeEdad = [
  { label: 'Entre los 14 y 18 años', value: '14-18', count: 0, color: 'bg-blue-600' },
  { label: 'Entre los 19 y 25 años', value: '19-25', count: 0, color: 'bg-teal-500' },
  { label: 'Entre los 26 y 45 años', value: '26-45', count: 0, color: 'bg-blue-400' },
  { label: 'Mayor de 45 años', value: '45+', count: 0, color: 'bg-yellow-500' },
];

const rangoDeEdadSeleccionado = ref(null);

const registrarEdad = () => {
  if (rangoDeEdadSeleccionado.value !== null) {
    const rangoSeleccionado = rangosDeEdad.find(rango => rango.value === rangoDeEdadSeleccionado.value);
    if (rangoSeleccionado) {
      rangoSeleccionado.count += 1;
      rangoDeEdadSeleccionado.value = null;
    }
  }
};

const obtenerTotal = () => rangosDeEdad.reduce((suma, rango) => suma + rango.count, 0);

const obtenerPorcentaje = (count) => {
  const total = obtenerTotal();
  return total > 0 ? Math.round((count / total) * 100) : 0;
};
</script>

<template>
  <div class="max-w-md mx-auto p-4">
    <h2 class="text-lg font-semibold mb-4">¿En qué rango de edad se encuentra?</h2>
    <div class="space-y-2">
      <label v-for="rango in rangosDeEdad" :key="rango.value" class="block">
        <input
          type="radio"
          :value="rango.value"
          v-model="rangoDeEdadSeleccionado"
          class="hidden peer"
        />
        <div
          :class="[rango.color, 'peer-checked:ring-2 peer-checked:ring-offset-2 peer-checked:ring-blue-500 cursor-pointer p-2 rounded-md']"
        >
          {{ rango.label }}
        </div>
      </label>
    </div>
    <button
      @click="registrarEdad"
      class="mt-4 bg-red-500 text-white px-4 py-2 rounded-md"
    >
      Registrar
    </button>

    <div v-if="obtenerTotal() > 0" class="mt-6">
      <h3 class="text-lg font-semibold mb-2">Resultados parciales</h3>
      <div v-for="rango in rangosDeEdad" :key="rango.value" class="mb-2">
        <div class="flex justify-between items-center">
          <span>{{ rango.label }}</span>
          <span>{{ obtenerPorcentaje(rango.count) }}%</span>
        </div>
        <div class="w-full bg-gray-200 rounded-full h-2.5">
          <div
            :class="[rango.color, 'h-2.5 rounded-full']"
            :style="{ width: `${obtenerPorcentaje(rango.count)}%` }"
          ></div>
        </div>
      </div>
    </div>

    <button
      @click="rangoDeEdadSeleccionado.value = null"
      class="mt-4 bg-gray-500 text-white px-4 py-2 rounded-md"
    >
      Registrar nuevamente
    </button>
  </div>
</template>

<style>
  /* Añadir estilos personalizados aquí si es necesario */
</style>
