<template>
  <div class="min-h-screen bg-gray-100">
    <Header :cart="cart" @update-cart="handleCartUpdate" />
    <main class="max-w-6xl mx-auto p-4">
      <ProductList @add-to-cart="addToCart" />
    </main>
  </div>
</template>

<script setup>
import Header from './components/Header.vue'
import ProductList from './components/ProductList.vue'
import { ref } from 'vue'

const cart = ref([])

function addToCart(produk) {
  const existing = cart.value.find(item => item.id === produk.id)
  if (existing) {
    existing.quantity++
  } else {
    cart.value.push({ ...produk, quantity: 1 })
  }
}

function handleCartUpdate(updatedCart) {
  cart.value = updatedCart
}

</script>
