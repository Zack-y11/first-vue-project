<script setup>
import { ref, computed } from 'vue';

// Get current date
const actual = new Date();

// Reactive reference for birth date input
const birthDate = ref('');

// Compute age based on birth date
const age = computed(() => {
    if(!birthDate.value) return null;
    const birthDateObj = new Date(birthDate.value); 
    
    // Check if date is in the future
    if(birthDateObj > actual) return 'Invalid date';

    // Calculate age in years
    let years = actual.getFullYear() - birthDateObj.getFullYear();
    
    // Adjust age if birthday hasn't occurred this year
    const monthDiff = actual.getMonth() - birthDateObj.getMonth();
    if(monthDiff < 0 || (monthDiff === 0 && actual.getDate() < birthDateObj.getDate())) {
        years--;
    }
    return years;
});

// Determine if person is adult (18 or older)
const isAdult = computed(() => {
    if (typeof age.value === 'number') {
        return age.value >= 18;
    }
    return null;
});
</script>

<template>
<div class="max-w-md mx-auto p-6 bg-purple-50 rounded-lg shadow-lg">
    <h1 class="text-2xl font-bold text-purple-800 mb-4">Compute your age</h1>
    <form class="space-y-4">
        <div class="mb-4">
            <label class="block text-purple-700 mb-2">Enter your birth date:</label>
            <input
                v-model="birthDate"
                type="date" 
                name="fecha_user"
                class="w-full p-2 border border-purple-300 rounded-md focus:ring-2 focus:ring-purple-500 focus:border-purple-500" 
            />
        </div>
        
        <div v-if="birthDate" class="mt-4 p-4 bg-purple-100 rounded-lg">
            <p class="text-lg">
                <span class="font-semibold">Your age: </span>
                <span v-if="age !== null && age !== 'Invalid date'" class="text-indigo-600 font-bold">{{ age }} years</span>
                <span v-else-if="age === 'Invalid date'" class="text-pink-600">Future dates are not valid</span>
            </p>
            
            <p v-if="isAdult === true" class="mt-2 text-emerald-600 font-medium">
                Eres mayor de edad
            </p>
            <p v-else-if="isAdult === false" class="mt-2 text-amber-600 font-medium">
                Eres menor de edad
            </p>
        </div>
    </form>
</div>
</template>
