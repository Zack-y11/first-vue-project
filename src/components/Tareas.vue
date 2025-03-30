<script setup>
import { ref } from "vue";

const Listatareas = ref([
  { id: 1, task: "wake up", status: false },
  { id: 2, task: "run", status: false },
  { id: 3, task: "breakfast", status: false },
  { id: 4, task: "go to university", status: false },
]);
</script>

<template>
<h1 class="text-3xl font-bold text-center mb-6 text-blue-600">Lista de Tareas</h1>
<ul class="max-w-md mx-auto space-y-3">
    <li v-for="tarea in Listatareas" 
            :key="tarea.id"
            class="flex items-center justify-between p-4 bg-white rounded-lg shadow-md">
        <span class="text-gray-700">{{ tarea.task }}</span>
        <div class="flex items-center gap-2">
            <label class="flex items-center">
                <input 
                    v-model="tarea.status" 
                    type="checkbox"
                    class="w-4 h-4 text-blue-600 rounded focus:ring-blue-500"
                />
            </label>
            <span 
                v-if="tarea.status" 
                class="text-green-600 font-medium"
            > completado</span>
            <span 
                v-else 
                class="text-red-600 font-medium"
            > no completado</span>
        </div>
    </li>
</ul>
</template>
