<!-- EJERCICIO 1 -->
<!-- NUMERO ENTRE 1 - 251 -->
<!-- DATA: NOMBRE, IMAGEN, ALTURA, PESO, TIPO, ESTADISTICAS -->
<script setup>
import { ref, computed, onMounted } from "vue";
import axios from "axios";

const pokemon = ref(null);
const error = ref("");
const isShiny = ref(false);

const tipo = computed(() => {
  return pokemon.value?.types?.map((tipo) => tipo.type.name) || [];
});

const getRandomPokemon = async () => {
  error.value = "";
  pokemon.value = null;

  const randomId = Math.floor(Math.random() * 251) + 1;

  try {
    const res = await axios.get(
      `https://pokeapi.co/api/v2/pokemon/${randomId}`
    );

    pokemon.value = res.data;
  } catch (err) {
    error.value = "Error al buscar el pokemon";
    console.log(err);
  }
};

// Load random pokemon on component mount
onMounted(() => {
  getRandomPokemon();
});
</script>

<template>
    <div
      class="container mx-auto p-8 max-w-lg bg-gradient-to-br from-gray-50 to-gray-100 rounded-lg shadow-lg"
    >
      <div class="mb-6">
        <button
          @click="getRandomPokemon()"
          class="bg-gradient-to-r from-blue-500 to-blue-600 text-white px-6 py-4 rounded-lg hover:from-blue-600 hover:to-blue-700 w-full transition duration-300 font-semibold"
        >
          Generate a random pokemon
        </button>
      </div>

      <div
        v-if="error"
        class="text-red-500 text-center p-4 bg-red-50 rounded-lg mb-4 border border-red-200"
      >
        {{ error }}
      </div>

      <div
        v-if="pokemon && pokemon.sprites"
        class="bg-white p-6 rounded-lg shadow-md hover:shadow-xl transition duration-300"
      >
        <div class="flex flex-col items-center">
          <div class="flex flex-col items-center gap-4">
            <img
              :src="isShiny ? pokemon.sprites.front_shiny : pokemon.sprites.front_default"
              :alt="pokemon.name"
              class="w-56 h-56 object-contain hover:scale-110 transition duration-300"
            />
            <p class="text-red-500" v-if="isShiny && pokemon.sprites?.front_shiny == null">No shiny version</p>
            <button
              @click="isShiny = !isShiny"
              class="px-4 py-2 bg-blue-500 text-white rounded-lg hover:bg-yellow-600 transition duration-300"
            >
              {{ isShiny ? 'Show Normal' : 'Show Shiny' }}
            </button>
          </div>
          <h2 class="text-3xl font-bold mt-4 capitalize text-gray-800">
            {{ pokemon.name }}
          </h2>

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
                'bg-gray-200 text-gray-800': ![
                  'grass',
                  'fire',
                  'water',
                  'electric',
                  'poison',
                ].includes(t),
              }"
            >
              {{ t }}
            </span>
          </div>

          <div class="mt-6 w-full">
            <h3 class="font-bold text-xl mb-4 text-gray-700">Stats:</h3>
            <div
              v-for="stat in pokemon.stats"
              :key="stat.stat.name"
              class="mb-3"
            >
              <div class="flex justify-between text-sm mb-1">
                <span class="capitalize font-semibold">{{
                  stat.stat.name
                }}</span>
                <span class="font-medium">{{ stat.base_stat }}/200</span>
              </div>
              <div class="w-full bg-gray-200 rounded-full h-3">
                <div
                  class="bg-gradient-to-r from-blue-400 to-blue-600 h-3 rounded-full transition-all duration-500"
                  :style="{ width: `${(stat.base_stat / 200) * 100}%` }"
                ></div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
</template>
