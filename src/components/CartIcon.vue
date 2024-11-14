<script setup>
import { inject } from 'vue'

defineProps({
  cartItems: Object,
})

const toggleDrawer = inject('toggleDrawer')
</script>

<template>
  <div v-if="cartItems.length > 0">
    <button class="btn d-flex align-items-center" @click="toggleDrawer">
      <i class="bi bi-bag fs-15 p-1"></i>
      <span class="badge rounded-pill bg-danger">{{ cartItems.length }}</span>
    </button>
  </div>
</template>

<style scoped>
.fs-15 {
  font-size: 1.5rem;
}
</style>
