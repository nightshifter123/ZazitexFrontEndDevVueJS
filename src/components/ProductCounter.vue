<script setup>
import { inject } from 'vue'

const props = defineProps({
  product: Object,
})

const updateCart = inject('updateCart')

const clickPlus = () => {
  checkAndUpdate(++props.product.buy_qty)
}
const clickMinus = () => {
  checkAndUpdate(--props.product.buy_qty)
}
const keyUp = (event) => {
  const current = Number(event.target.value)
  checkAndUpdate(current)
}

const checkAndUpdate = (current) => {
  const minValue = 1
  const maxValue = props.product.quantity
  if (current <= minValue) {
    props.product.buy_qty = minValue
  }
  if (current >= maxValue) {
    props.product.buy_qty = maxValue
  }
  updateCart()
}
</script>

<template>
  <div class="input-group flex-nowrap product-counter" role="group">
    <button type="button" class="btn" @click="clickMinus">
      <i class="bi bi-dash-lg"></i>
    </button>
    <input type="number" class="form-control text-center" v-model="product.buy_qty" min="1" :max="product.quantity" @keyup="keyUp"/>
    <button type="button" class="btn" @click="clickPlus">
      <i class="bi bi-plus-lg"></i>
    </button>
  </div>
</template>

<style scoped>
/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type=number] {
  -moz-appearance: textfield;
}

.product-counter input,
.product-counter button {
  outline: none !important;
  border: none !important;
  box-shadow: none !important;
}

.product-counter input {
  background-color: var(--bs-gray-300);
  width: 52px;
}

.product-counter .btn {
  background-color: var(--bs-gray-300);
}
</style>
