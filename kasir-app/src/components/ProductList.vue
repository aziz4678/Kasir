<template>
  <div class="grid grid-cols-2 md:grid-cols-4 gap-4 p-4">
    <div
      v-for="product in products"
      :key="product.id"
      class="bg-white p-4 rounded shadow hover:shadow-lg transition flex flex-col"
    >
      <img
        :src="product.image"
        :alt="product.title"
        class="w-full h-40 object-contain mb-2"
      />
      <h2 class="font-semibold text-base mb-1">{{ product.title }}</h2>
      <p class="text-sm text-gray-600 mb-3">
        Rp {{ product.price.toLocaleString() }}
      </p>

      <button
        @click="$emit('add-to-cart', product)"
        class="mt-auto px-3 py-2 bg-blue-500 text-white text-sm rounded flex items-center justify-center gap-1 hover:bg-blue-600 transition"
      >
        <ShoppingCartIcon class="w-4 h-4" />
        Tambah ke Keranjang
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { ShoppingCartIcon } from "@heroicons/vue/24/solid";

defineEmits(["add-to-cart"]);

const products = ref([]);

onMounted(async () => {
  const res = await fetch("http://localhost:3001/products");
  products.value = await res.json();
});
</script>
