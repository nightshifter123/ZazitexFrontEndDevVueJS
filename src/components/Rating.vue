<script setup>
defineProps({
  rating: Number,
})
</script>

<template>
  <div class="d-flex flex-nowrap">
    <span class="fs-5 me-1" v-for="i in [1, 2, 3, 4, 5]">
      <i class="stars-yellow" :class="i > rating ? 'bi bi-star' : 'bi bi-star-fill'"></i>
    </span>
  </div>
</template>

<style scoped>
.stars-yellow {
  color: #41B883;
}
</style>
