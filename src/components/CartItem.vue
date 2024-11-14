<script setup>
import { inject, computed } from 'vue'
import Rating from './Rating.vue'
import ProductCounter from "./ProductCounter.vue"

const props = defineProps({
  product: Object,
})

const priceSum = computed(() => {
  return (props.product.price * props.product.buy_qty).toFixed(2)
})
const removeFromCart = inject('removeFromCart')
</script>

<template>
  <tr>
    <td class="align-middle">
      <div class="d-flex flex-row">
        <img :src="product.image" class="cart-img me-2" :alt="product.title"/>
        <div class="d-flex flex-column product-info">
          <div class="fw-bold mb-2">{{ product.title }}</div>
          <div class="fw-light mb-2">{{ product.manufacturer }}</div>
          <div class="d-flex mb-2">
            <span class="fw-bold fs-6">{{ product.price }} MDL</span><span class="fw-light ms-1 ps-1">Qty: {{ product.quantity }}</span>
          </div>
          <div class="mb-2">
            <Rating :rating="product.rating"/>
          </div>
        </div>
      </div>
    </td>
    <td class="align-middle">
      <ProductCounter :product="product" :priceSum="priceSum"/>
    </td>
    <td class="align-middle">
      <span class="fw-bold fs-4">{{ priceSum }}</span>
    </td>
    <td class="align-middle">
      <button class="btn cart-btn fs-4" @click="removeFromCart(product)"><i class="bi bi-x-circle"></i></button>
    </td>
  </tr>
</template>

<style scoped>
.cart-img {
  border-radius: var(--bs-card-inner-border-radius);
  width: 138px;
  height: 138px;
  object-fit: cover;
}

.cart-btn:hover {
  color: rgba(var(--bs-danger-rgb), var(--bs-bg-opacity));
}

.product-info {
  line-height: 1.2;
}
</style>
