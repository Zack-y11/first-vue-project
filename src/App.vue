<script setup>
import Navbar from './components/Navbar.vue';
import tareas from './components/Tareas.vue'
import calculadora from './components/calculadora.vue';
</script>

<template>
  <div>
    <div class="min-h-screen bg-gray-100 py-12 px-4 sm:px-6 lg:px-8">
      <div class="max-w-7xl mx-auto">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
          <div class="bg-white rounded-lg shadow-lg p-6 hover:shadow-xl transition duration-300">
            <h2 class="text-2xl font-bold mb-4 text-gray-800">Ejercicio 1: Calculadora</h2>
            <calculadora />
          </div>
          
          <div class="bg-white rounded-lg shadow-lg p-6 hover:shadow-xl transition duration-300">
            <h2 class="text-2xl font-bold mb-4 text-gray-800">Ejercicio 2: Lista de Tareas</h2>
            <tareas />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<style scoped>

</style>
