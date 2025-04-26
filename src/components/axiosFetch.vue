<script setup>
import { ref, computed } from "vue";
import axios from "axios";

const nombrePokemon = ref("");
const pokemon = ref(null);
const error = ref("");

const tipo = computed(() => {
  return pokemon.value?.types?.map((tipo) => tipo.type.name) || [];
});

const buscarPokemon = async () => {
  error.value = "";
  pokemon.value = null;

  if (!nombrePokemon.value.trim()) {
    error.value = "El nombre no puede estar vacio";
    return;
  }

  try {
    const res = await axios.get(
      `https://pokeapi.co/api/v2/pokemon/${nombrePokemon.value.toLowerCase()}`
    );
    //if (!res.ok) throw new Error("El pokemon no ha sido encontrado");
    //const data = await res.json();
    pokemon.value = res.data;
  } catch (err) {
    error.value = err.response?.status === 404
    ? "Pokemon No encontrado"
    : "Error al buscar el pokemon"
    console.log(err);
  }
}
</script>

<template>
    <div class="container mx-auto p-8 max-w-lg bg-gradient-to-br from-gray-50 to-gray-100 rounded-lg shadow-lg">
        <!-- Search Section -->
        <div class="mb-6">
            <input
                type="text"
                v-model="nombrePokemon"
                placeholder="Escribe el nombre del pokemon"
                class="border-2 border-gray-300 rounded-lg p-4 w-full mb-4 focus:outline-none focus:border-blue-500 transition duration-300"
            />
            <button
                @click="buscarPokemon"
                class="bg-gradient-to-r from-blue-500 to-blue-600 text-white px-6 py-4 rounded-lg hover:from-blue-600 hover:to-blue-700 w-full transition duration-300 font-semibold"
            >
                Buscar Pokemon
            </button>
        </div>

        <!-- Error Message -->
        <div v-if="error" class="text-red-500 text-center p-4 bg-red-50 rounded-lg mb-4 border border-red-200">
            {{ error }}
        </div>

        <!-- Pokemon Details -->
        <div v-if="pokemon && pokemon.sprites" class="bg-white p-6 rounded-lg shadow-md hover:shadow-xl transition duration-300">
            <div class="flex flex-col items-center">
                <img 
                    :src="pokemon.sprites.front_default" 
                    :alt="pokemon.name"
                    class="w-56 h-56 object-contain hover:scale-110 transition duration-300"
                >
                <h2 class="text-3xl font-bold mt-4 capitalize text-gray-800">
                    {{ pokemon.name }}
                </h2>
                
                <!-- Types -->
                <div class="mt-4 flex gap-3">
                    <span 
                        v-for="(t, index) in tipo" 
                        :key="index"
                        class="px-4 py-2 rounded-full text-sm font-bold shadow-sm"
                        :class="{
                            'bg-green-200 text-green-800': t === 'grass',
                            'bg-red-200 text-red-800': t === 'fire',
                            'bg-blue-200 text-blue-800': t === 'water',
                            'bg-yellow-200 text-yellow-800': t === 'electric',
                            'bg-purple-200 text-purple-800': t === 'poison',
                            'bg-gray-200 text-gray-800': !['grass', 'fire', 'water', 'electric', 'poison'].includes(t)
                        }"
                    >
                        {{ t }}
                    </span>
                </div>

                <!-- Stats -->
                <div class="mt-6 w-full">
                    <h3 class="font-bold text-xl mb-4 text-gray-700">Stats:</h3>
                    <div v-for="stat in pokemon.stats" :key="stat.stat.name" class="mb-3">
                        <div class="flex justify-between text-sm mb-1">
                            <span class="capitalize font-semibold">{{ stat.stat.name }}</span>
                            <span class="font-medium">{{ stat.base_stat }}/200</span>
                        </div>
                        <div class="w-full bg-gray-200 rounded-full h-3">
                            <div 
                                class="bg-gradient-to-r from-blue-400 to-blue-600 h-3 rounded-full transition-all duration-500"
                                :style="{ width: `${(stat.base_stat/200)*100}%` }"
                            ></div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>