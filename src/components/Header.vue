<script setup>
import CartIcon from './CartIcon.vue';

defineProps({
  cartItems: Object,
})
</script>

<template>
  <nav class="navbar bg-light">
    <div class="container">
      <a class="navbar-brand" href="/">
        <img src="/logo.svg" alt="Logo" width="52" height="52" class="d-inline-block align-text-top"/>
      </a>
      <CartIcon :cartItems="cartItems" />
    </div>
  </nav>
</template>

<style scoped>
.navbar {
  box-shadow: 0 4px 4px rgba(33, 66, 120, 0.15) !important;
}
</style>
