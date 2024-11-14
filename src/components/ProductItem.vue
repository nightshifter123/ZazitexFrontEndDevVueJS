<script setup>
import {ref, inject} from 'vue'

const props = defineProps({
  product: Object,
  productsLoaded: Boolean,
})

let setQty = ref(1)

const addToCart = inject('addToCart')

const keyUp = (event) => {
  const current = Number(event.target.value)
  const minValue = 1
  const maxValue = props.product.quantity
  if (current <= minValue) {
    setQty.value = minValue
  }
  if (current >= maxValue) {
    setQty.value = maxValue
  }
}
</script>

<template>
  <div v-if="productsLoaded" class="col-xl-3 col-md-4 col-6 mb-2">
    <div class="card h-100 m-1">
      <div class="card-head">
        <img :src="product.image" class="card-img-top" :alt="product.title"/>
        <div class="rating">{{ product.rating.toFixed(1) }}</div>
      </div>
      <div class="card-body">
        <div class="h5 fw-bold">{{ product.title }}</div>
        <div class="fw-light">{{ product.manufacturer }}</div>
        <div class="d-flex align-items-center">
          <div class="h3 fw-bold flex-grow-1">{{ product.price }} MDL</div>
          <div class="mb-2 ms-2 ps-2 fs-6 fw-light flex-grow-0">Qty: {{ product.quantity }}</div>
        </div>
      </div>
      <div class="card-footer bg-white d-flex">
        <div class="input-group flex-nowrap product-cart mt-2 mb-2" role="group">
          <button type="button" class="btn btn-outline-success flex-grow-1" @click="addToCart(product, setQty)">În Coș</button>
          <div class="form-control flex-grow-0 form-wrapper">
            <input type="number" class="form-control p-0 text-center" v-model="setQty" min="1" :max="product.quantity" @keyup="keyUp"/>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div v-else class="col-xl-3 col-md-4 col-6 mb-2">
    <div class="card h-100 m-1">
      <div class="card-head">
        <div class="card-img-top img-loading"/>
      </div>
      <div class="card-body">
        <div class="h5 fw-bold title-loading"/>
        <div class="extra-info-loading"/>
      </div>
      <div class="card-footer bg-white d-flex">
        <div class="flex-grow-1 btn-loading"/>
      </div>
    </div>
  </div>
</template>

<style scoped>
.card-head {
  border-bottom: var(--bs-card-border-width) solid var(--bs-card-border-color);
}

.card-head img {
  height: 238px;
  object-fit: cover;
}

.rating {
  margin: 11px 15px;
  width: 54px;
  height: 54px;
  position: absolute;
  top: 0;
  right: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #41B883;
  color: #ffffff;
  border-radius: 50%;
  font-weight: 500;
  font-size: 1.4rem;
  line-height: 1;
}

.product-cart input {
  outline: none !important;
  border: none !important;
  box-shadow: none !important;
}

.product-cart .form-control.form-wrapper {
  width: 76px;
}

.product-cart input[type=number]::-webkit-inner-spin-button,
.product-cart input[type=number]::-webkit-outer-spin-button {
  opacity: 1;
}

.product-cart input.form-control {
  width: 52px;
}

/* Loading Content */
.card-head .img-loading,
.card-body .title-loading,
.card-body .extra-info-loading,
.card-footer .btn-loading {
  background-color: var(--bs-gray-300);
}

.card-body .title-loading,
.card-body .extra-info-loading,
.card-footer .btn-loading {
  border-radius: var(--bs-card-inner-border-radius);
}

.card-head .img-loading {
  height: 238px;
}

.card-body .title-loading {
  height: 48px;
}

.card-body .extra-info-loading {
  height: 66px;
}

.card-footer .btn-loading {
  height: 50px;
}
</style>
