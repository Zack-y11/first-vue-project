<script setup>
import { ref, computed } from "vue";

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
    const res = await fetch(
      `https://pokeapi.co/api/v2/pokemon/${nombrePokemon.value.toLowerCase()}`
    );
    if (!res.ok) throw new Error("El pokemon no ha sido encontrado");
    const data = await res.json();
    pokemon.value = data;
  } catch (err) {
    error.value = "Pokemon no encontrado";
    console.log(err);
  }
}
</script>

<template>
  <div class="container mx-auto p-6 max-w-md bg-gray-100 rounded-lg shadow-md">
    <input
      type="text"
      v-model="nombrePokemon"
      placeholder="Escribe el nombre del pokemon"
      class="border-2 border-gray-300 rounded-md p-2 justify-center w-full mb-4"
    />
    <button
      @click="buscarPokemon"
      class="bg-blue-500 text-white px-4 py-2 rounded-md hover:bg-blue-600"
    >
      Buscar Pokemon
    </button>
    <div v-if="error" class="text-red-500 mt-2">{{ error }}</div>
    <div v-if="pokemon && pokemon.sprites" class="mt-4">
        <img :src="pokemon.sprites.front_default" alt="imagen">
        <h2 class="text-xl font-bold mt-2">{{ pokemon.name }}</h2>
        <p>Types: {{ tipo.join(", ") }}</p>
    </div>
    <p v-if="error" class="text-red">
        {{ error }}
    </p>
  </div>
</template>