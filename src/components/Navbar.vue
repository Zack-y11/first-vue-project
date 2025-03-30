<script setup>
import { ref } from "vue";
const name = "soy un componente";

//reactive counter
const counter = ref(0);

//redirect webpages
const url = ref("https://isaacmedrano.tech")

//showing elements in array using ref
const elements = ref(["apple", "banana", "strawberry"]);

//
const counting = ref(5)


const increment = () => {
  counter.value++;
};

defineProps({
  title: String,
  description: String,
});
</script>

<template>

    <div class="w-50 h-10 bg-indigo-500">
        <h2 v-if="conteo > 10">Si es mayor que 0</h2>
        <h2 v-else>No es mayor que 10</h2>
    </div>

    <div class="bg-blue-400 ">
        <a v-bind:href="url">Check My Website</a>
    </div>
    <div class="w-30 h-auto bg-amber-600">
        <ul>
            <li v-for="e in elements">{{ e }}</li>
        </ul>

    </div>
  <!-- <div>
    <h2>{{ title }}</h2>
    <p>{{ description }}</p>

    <h1>Contador: {{ counter }}</h1>
    <button @click="increment">Click Please</button>
  </div> -->
</template>
