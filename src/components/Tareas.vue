<script setup>
import { ref } from "vue";

// Initial array of tasks with their properties
const Listatareas = ref([
    { id: 1, task: "wake up", status: false },
    { id: 2, task: "run", status: false },
    { id: 3, task: "breakfast", status: false },
    { id: 4, task: "go to university", status: false },
]);
</script>

<template>
<!-- Main title -->
<h1 class="text-3xl font-bold text-center mb-6 text-purple-600">Lista de Tareas</h1>

<!-- Task list container -->
<ul class="max-w-md mx-auto space-y-3">
        <!-- Individual task item -->
        <li v-for="tarea in Listatareas" 
                        :key="tarea.id"
                        class="flex items-center justify-between p-4 bg-gray-50 rounded-lg shadow-md hover:shadow-lg">
                <!-- Task name -->
                <span class="text-gray-800">{{ tarea.task }}</span>
                <!-- Status controls container -->
                <div class="flex items-center gap-2">
                        <!-- Checkbox input -->
                        <label class="flex items-center">
                                <input 
                                        v-model="tarea.status" 
                                        type="checkbox"
                                        class="w-4 h-4 text-purple-600 rounded focus:ring-purple-500"
                                />
                        </label>
                        <!-- Status text indicator -->
                        <span 
                                v-if="tarea.status" 
                                class="text-emerald-600 font-medium"
                        > completado</span>
                        <span 
                                v-else 
                                class="text-orange-600 font-medium"
                        > no completado</span>
                </div>
        </li>
</ul>
</template>
