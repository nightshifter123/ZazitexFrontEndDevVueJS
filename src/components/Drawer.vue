<script setup>
import { watch, toRef, inject } from 'vue'

const props = defineProps({
  drawerVisible: Boolean,
})

const toggleDrawer = inject('toggleDrawer')
const drawerVisibleRef = toRef(props, "drawerVisible")

watch(drawerVisibleRef, () => {
  document.body.classList.toggle('no-scroll')
})
</script>

<template>
  <div class="position-fixed top-0 start-0 h-100 w-100 drawer-scrim z-1099 collapse" :class="drawerVisible ? 'show' : ''" @click="toggleDrawer"/>
  <div class="position-fixed top-0 end-0 h-100 bg-white col-xl-5 col-lg-6 col-12 z-1100 collapse" :class="drawerVisible ? 'show' : ''">
    <slot></slot>
  </div>
</template>

<style scoped>
.drawer-scrim {
  background-color: rgba(0, 0, 0, .5);
  backdrop-filter: blur(10px);
}
</style>
