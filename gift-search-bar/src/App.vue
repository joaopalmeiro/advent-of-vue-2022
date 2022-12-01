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

// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch
// Debouncing: a technique to delay a function's execution until
// a specific amount of time has elapsed since the last call.
// https://dmitripavlutin.com/javascript-fetch-async-await/#2-fetching-json
// https://developer.mozilla.org/en-US/docs/Web/API/Response/json#return_value
const findProducts = debounce(async term => {
  try {
    // Clear
    if (term.length === 0) return (products.value = [])

    isLoading.value = true

    const response = await fetch(`https://dummyjson.com/products/search?q=${term}&limit=5`)
    const data = await response.json()
    // console.log(data)

    products.value = data.products
  } catch (error) {
    console.error(error)
    alert('There is a problem fetching the data!')
  } finally {
    isLoading.value = false
  }
}, 300)

// https://vuejs.org/api/reactivity-core.html#watch
// "Watches one or more reactive data sources and
// invokes a callback function when the sources change."
watch(searchTerm, newTerm => findProducts(newTerm))
</script>

<template>
  <div class="w-full h-full flex flex-col gap-5 justify-center items-center">
    <h1 class="text-4xl font-bold">Gift Search Bar</h1>
    <!-- https://vuejs.org/guide/essentials/forms.html -->
    <input
      type="text"
      class="p-2 border-2 border-gray-dark disabled:opacity-40"
      v-model="searchTerm"
      placeholder="Start typing..."
      :disabled="isLoading"
    />
    <p v-if="isLoading" class="text-lg text-center">Loading…</p>
    <!-- https://tailwindcss.com/docs/list-style-type -->
    <!-- https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dl -->
    <!-- https://vuejs.org/guide/essentials/list.html#v-for-on-template -->
    <ul v-else-if="!isLoading && products.length > 0" class="list-disc">
      <li v-for="product in products" :key="product.id">{{ product.title }} - ${{ product.price }}</li>
    </ul>
  </div>
</template>
