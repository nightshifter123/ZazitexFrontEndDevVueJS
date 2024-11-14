<script setup>
import CartItem from './CartItem.vue'
import { inject, computed } from 'vue'

const props = defineProps({
  cartItems: Object,
})

const toggleDrawer = inject('toggleDrawer')
const totalCost = computed(() => {
  return props.cartItems.reduce((total, item) => total + item.price * item.buy_qty, 0).toFixed(2)
})
</script>

<template>
  <div class="container p-4">
    <div class="d-flex justify-content-end">
      <button type="button" class="btn-close" aria-label="Close" @click="toggleDrawer"/>
    </div>
    <div class="h2 fw-bold mb-3">Coș de cumpărături</div>
    <div class="table-wrapper">
      <table class="table">
        <thead class="text-center">
        <tr>
          <th scope="col">Produsul</th>
          <th scope="col">Cantitatea</th>
          <th scope="col">Suma</th>
          <th scope="col">Șterge</th>
        </tr>
        </thead>
        <tbody>
        <CartItem v-for="item in cartItems" :product="item" :key="item.id" class="cart-item"/>
        </tbody>
      </table>
    </div>
    <div class="d-flex justify-content-end fs-5">
      <span>Suma totală: <span class="fw-bold fs-2">{{ totalCost }}</span> MDL</span>
    </div>
  </div>
</template>

<style scoped>
.table-wrapper {
  max-height: 75vh;
  overflow-y: auto;
}

th {
  color: var(--bs-gray-600);
  font-weight: 300;
}
</style>
