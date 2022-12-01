<!-- Source: https://github.com/Advent-Of-Vue/2022-gift-search-bar/tree/solution by Maciej Pędzich -->

<!-- https://vuejs.org/api/sfc-script-setup.html -->
<script setup>
import { ref, watch } from 'vue'
import { debounce } from 'debounce'

// https://vuejs.org/api/reactivity-core.html#ref
// "Takes an inner value and returns a reactive and mutable ref object,
// which has a single property .value that points to the inner value."
const searchTerm = ref('')
const isLoading = ref(false)
const products = ref([])

const findProducts = async term => {}

// https://vuejs.org/api/reactivity-core.html#watch
// "Watches one or more reactive data sources and
// invokes a callback function when the sources change."
watch(searchTerm, newTerm => findProducts(newTerm))
</script>

<template>
  <div class="w-full h-full flex flex-col gap-5 justify-center items-center">
    <h1 class="text-4xl font-bold">Gift Search Bar</h1>
    <!-- https://vuejs.org/guide/essentials/forms.html -->
    <input type="text" class="p-2 border-2 border-gray-dark" v-model="searchTerm" placeholder="Start typing..." />
    <ul class="list-disc">
      <li>Display suggestions here</li>
    </ul>
  </div>
</template>
