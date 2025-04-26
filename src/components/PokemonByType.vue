<script setup>
import { ref, computed } from "vue";
import axios from "axios";

const selectedType = ref("");
const pokemons = ref([]);
const error = ref("");
const loading = ref(false);

const types = ["bug", "normal", "rock"];

const fetchPokemonsByType = async () => {
  if (!selectedType.value) return;

  loading.value = true;
  error.value = "";
  pokemons.value = [];

  try {
    const response = await axios.get(`https://pokeapi.co/api/v2/type/${selectedType.value}`);
    const pokemonUrls = response.data.pokemon.slice(0, 5).map(p => p.pokemon.url);

    const pokemonDetails = await Promise.all(
      pokemonUrls.map(url => axios.get(url))
    );

    pokemons.value = pokemonDetails.map(response => response.data);
  } catch (err) {
    error.value = "Error fetching Pokémon";
    console.error(err);
  } finally {
    loading.value = false;
  }
};
</script>

<template>
  <div class="container mx-auto p-8 max-w-lg bg-gradient-to-br from-gray-50 to-gray-100 rounded-lg shadow-lg">
    <div class="mb-6">
      <h3 class="mb-4">Ejercicio 2: seleccionar por tipo</h3>
      <select
        v-model="selectedType"
        @change="fetchPokemonsByType"
        class="w-full p-4 border-2 border-gray-300 rounded-lg focus:outline-none focus:border-blue-500 transition duration-300"
      >
        <option value="">Select a type</option>
        <option v-for="type in types" :key="type" :value="type">
          {{ type.charAt(0).toUpperCase() + type.slice(1) }}
        </option>
      </select>
    </div>

    <div v-if="loading" class="text-center text-gray-600 my-4">
      Loading Pokémon...
    </div>

    <div v-if="error" class="text-red-500 text-center p-4 bg-red-50 rounded-lg mb-4 border border-red-200">
      {{ error }}
    </div>

    <div v-if="pokemons.length" class="space-y-4">
      <div v-for="pokemon in pokemons" :key="pokemon.id" 
           class="bg-white p-4 rounded-lg shadow-md hover:shadow-xl transition duration-300">
        <div class="flex items-center space-x-4">
          <img 
            :src="pokemon.sprites.front_default" 
            :alt="pokemon.name"
            class="w-24 h-24 object-contain"
          >
          <div>
            <h2 class="text-xl font-bold capitalize">{{ pokemon.name }}</h2>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>