<template>
  <header class="bg-white shadow p-4 flex justify-between items-center">
    <!-- Logo -->
    <div class="flex items-center space-x-2">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        class="w-6 h-6 text-blue-600"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13l-1.6 8H17M7 13L5.4 5M17 13l1.6 8M9 21a1 1 0 100-2 1 1 0 000 2zm6 0a1 1 0 100-2 1 1 0 000 2z"
        />
      </svg>
      <h1 class="text-xl font-bold text-gray-800">KasirApp</h1>
    </div>

    <!-- Cart Icon -->
    <div class="relative">
      <button
        @click="toggleDropdown"
        class="relative p-2 hover:bg-gray-100 rounded transition"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="w-6 h-6 text-gray-700"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13l-1.6 8H17M7 13L5.4 5M17 13l1.6 8M9 21a1 1 0 100-2 1 1 0 000 2zm6 0a1 1 0 100-2 1 1 0 000 2z"
          />
        </svg>
        <span
          v-if="cart.length"
          class="absolute -top-1 -right-1 bg-red-500 text-white text-xs font-bold px-1.5 py-0.5 rounded-full"
        >
          {{ cart.length }}
        </span>
      </button>

      <!-- Cart Dropdown -->
      <div
        v-if="dropdownOpen"
        class="absolute right-0 mt-2 w-80 bg-white border rounded-lg shadow-lg z-50"
      >
        <div
          v-if="cart.length === 0"
          class="p-4 text-gray-500 text-sm text-center"
        >
          Keranjang Kosong
        </div>

        <div v-else class="max-h-64 overflow-y-auto space-y-4 px-2 py-3">
          <div
            v-for="item in cart"
            :key="item.id"
            class="flex items-center justify-between bg-white shadow rounded-lg p-3"
          >
            <!-- Gambar dan Info Produk -->
            <div class="flex items-start space-x-3 w-2/3">
              <img
                :src="item.image"
                :alt="item.title"
                class="w-20 h-20 object-contain rounded"
              />
              <div class="flex flex-col">
                <p class="text-sm font-semibold text-gray-800 line-clamp-2">
                  {{ item.title }}
                </p>
                <p class="text-xs text-gray-500 mt-1">
                  Rp {{ (item.price * item.quantity).toLocaleString() }}
                </p>
              </div>
            </div>

            <!-- Kontrol Quantity -->
            <div class="flex items-center space-x-2">
              <button
                @click="decreaseQty(item)"
                class="w-7 h-7 flex items-center justify-center bg-gray-100 hover:bg-gray-200 rounded-full text-sm"
              >
                −
              </button>
              <span class="text-sm">{{ item.quantity }}</span>
              <button
                @click="increaseQty(item)"
                class="w-7 h-7 flex items-center justify-center bg-gray-100 hover:bg-gray-200 rounded-full text-sm"
              >
                +
              </button>
            </div>

            <!-- Tombol Hapus -->
            <button
              @click="removeItem(item.id)"
              class="text-red-500 hover:text-red-600 text-xl ml-2"
              title="Hapus item"
            >
              &times;
            </button>
          </div>
        </div>

        <div class="p-3 border-t text-right">
          <router-link
            to="/checkout"
            class="bg-blue-600 text-white px-4 py-2 text-sm rounded hover:bg-blue-700"
          >
            Checkout
          </router-link>
        </div>
      </div>
    </div>
  </header>
</template>

<script setup>
import { ref, watch } from "vue";
const props = defineProps(["cart"]);
const emit = defineEmits(["update-cart"]);

const dropdownOpen = ref(false);

function toggleDropdown() {
  dropdownOpen.value = !dropdownOpen.value;
}

function increaseQty(item) {
  item.quantity++;
  emit("update-cart", [...props.cart]);
}

function decreaseQty(item) {
  if (item.quantity > 1) {
    item.quantity--;
    emit("update-cart", [...props.cart]);
  } else {
    removeItem(item.id);
  }
}

function removeItem(id) {
  const updated = props.cart.filter((item) => item.id !== id);
  emit("update-cart", updated);
}

function handleClickOutside(event) {
  if (!event.target.closest("header")) {
    dropdownOpen.value = false;
  }
}

watch(dropdownOpen, (open) => {
  if (open) {
    document.addEventListener("click", handleClickOutside);
  } else {
    document.removeEventListener("click", handleClickOutside);
  }
});
</script>

<style scoped>
::-webkit-scrollbar {
  width: 6px;
}
::-webkit-scrollbar-thumb {
  background-color: rgba(0, 0, 0, 0.2);
  border-radius: 4px;
}
</style>
