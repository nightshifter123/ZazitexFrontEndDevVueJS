<script setup>
import ProductItem from './ProductItem.vue'

defineProps({
  products: Object,
  productsLoaded: Boolean,
})
</script>

<template>
  <div class="d-flex flex-row flex-wrap pt-3 pb-3">
    <ProductItem :productsLoaded="productsLoaded" v-for="product in products" :key="product.id" :product="product" />
  </div>
</template>

<style scoped>

</style>
