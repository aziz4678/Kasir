<template>
  <div class="grid grid-cols-2 md:grid-cols-4 gap-4 p-4">
    <div
      v-for="product in products"
      :key="product.id"
      class="bg-white p-4 rounded shadow hover:shadow-lg transition"
    >
      <img
        :src="product.image"
        :alt="product.title"
        class="w-full h-40 object-contain mb-2"
      />
      <h2 class="font-semibold text-base">{{ product.title }}</h2>
      <p class="text-sm text-gray-600">Rp {{ product.price.toLocaleString() }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const products = ref([])

onMounted(async () => {
  const res = await fetch('http://localhost:3001/products')
  products.value = await res.json()
})
</script>
