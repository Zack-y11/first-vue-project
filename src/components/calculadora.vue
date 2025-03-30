<script setup>
import { ref, computed } from 'vue';

const actual = new Date();
const birthDate = ref('');
const age = computed(() => {
    if(!birthDate.value) return null;
    const birthDateObj = new Date(birthDate.value); 
    if(birthDateObj > actual) return 'Invalid date';

    let years = actual.getFullYear() - birthDateObj.getFullYear();
    const monthDiff = actual.getMonth() - birthDateObj.getMonth();
    if(monthDiff < 0 || (monthDiff === 0 && actual.getDate() < birthDateObj.getDate())) {
        years--;
    }
    return years;
});
const isAdult = computed(() => {
    if (typeof age.value === 'number') {
        return age.value >= 18;
    }
    return null;
});
</script>

<template>
<div class="max-w-md mx-auto p-6 bg-white rounded-lg shadow-md">
    <h1 class="text-2xl font-bold text-gray-800 mb-4">Compute your age</h1>
    <form class="space-y-4">
        <div class="mb-4">
            <label class="block text-gray-700 mb-2">Enter your birth date:</label>
            <input
                v-model="birthDate"
                type="date" 
                name="fecha_user"
                class="w-full p-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500 focus:border-blue-500" 
            />
        </div>
        
        <div v-if="birthDate" class="mt-4 p-4 bg-gray-100 rounded-lg">
            <p class="text-lg">
                <span class="font-semibold">Your age: </span>
                <span v-if="age !== null && age !== 'Invalid date'" class="text-blue-600 font-bold">{{ age }} years</span>
                <span v-else-if="age === 'Invalid date'" class="text-red-600">Future dates are not valid</span>
            </p>
            
            <p v-if="isAdult === true" class="mt-2 text-green-600 font-medium">
                Eres mayor de edad
            </p>
            <p v-else-if="isAdult === false" class="mt-2 text-orange-600 font-medium">
                Eres menor de edad
            </p>
        </div>
    </form>
</div>
</template>
