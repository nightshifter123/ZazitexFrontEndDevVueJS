<script setup>
import { openDB, deleteDB } from 'idb'
import { ref, onBeforeMount, provide } from 'vue'
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'
import ProductList from './components/ProductList.vue'
import Drawer from './components/Drawer.vue'
import Cart from './components/Cart.vue'

import initProducts from './assets/products.json'

let productsLoaded = ref(false)
const cartItems = ref([])
let products = ref([{id: 1},{id: 2},{id: 3},{id: 4}])
const drawerVisible = ref(false)

const setupDB = async () => {
  const db = await openDB('ecommerceDB', 1, {
    upgrade(db) {
      let productsObjectStore = db.createObjectStore('products', { keyPath: 'id', autoIncrement: true })
      productsObjectStore.createIndex("title", "title", { unique: false })
      productsObjectStore.createIndex("price", "price", { unique: false })
      productsObjectStore.createIndex("rating", "rating", { unique: false })
      productsObjectStore.createIndex("manufacturer", "manufacturer", { unique: false })
      productsObjectStore.createIndex("image", "image", { unique: false })
      productsObjectStore.createIndex("quantity", "quantity", { unique: false })
      let cartObjectStore = db.createObjectStore('cart', { keyPath: 'id', autoIncrement: true })
      cartObjectStore.createIndex("title", "title", { unique: false })
      cartObjectStore.createIndex("price", "price", { unique: false })
      cartObjectStore.createIndex("rating", "rating", { unique: false })
      cartObjectStore.createIndex("manufacturer", "manufacturer", { unique: false })
      cartObjectStore.createIndex("image", "image", { unique: false })
      cartObjectStore.createIndex("quantity", "quantity", { unique: false })
      cartObjectStore.createIndex("buy quantity", "buy_qty", { unique: false })
    },
  })
  return db
}

const displayDB = async () => {
  const db = await setupDB()
  const txProducts = db.transaction('products', 'readonly')
  const allProducts = await txProducts.store.getAll()
  console.log('Products', allProducts)
  const txCart = db.transaction('cart', 'readonly')
  const allCart = await txCart.store.getAll()
  console.log('Cart', allCart)
}

const killDB = async() => {
  await deleteDB('ecommerceDB', {
    blocked() {},
  })
}

const loadProducts = async (db) => {
  const tx = db.transaction('products', 'readonly')
  let storedProducts = await tx.store.getAll()
  if (storedProducts.length === 0) {
    await saveProducts(db, initProducts)
  }
  products.value = storedProducts
}

const saveProducts = async (db, products) => {
  const tx = db.transaction('products', 'readwrite')
  for (const product of products) {
    await tx.store.put(product)
  }
  await tx.done
}

const loadCart = async (db) => {
  const tx = db.transaction('cart', 'readonly')
  cartItems.value = await tx.store.getAll()
}

const saveCart = async (db) => {
  const tx = db.transaction('cart', 'readwrite')
  await tx.store.clear()
  const plainCartItems = JSON.parse(JSON.stringify(cartItems.value))
  for (const item of plainCartItems) {
    await tx.store.put(item)
  }
  await tx.done
}

const updateCart = async () => {
  const db = await setupDB()
  await saveCart(db)
}

const addToCart = async (product, qty = 1) => {
  const existingItem = cartItems.value.find((item) => item.id === product.id)
  if (existingItem) {
    existingItem.buy_qty += qty
  } else {
    cartItems.value.push({ ...product, buy_qty: qty })
  }
  await updateCart()
}

const toggleDrawer = () => {
  drawerVisible.value = !drawerVisible.value
}

const removeFromCart = async (item) => {
  cartItems.value = cartItems.value.filter((cartItem) => cartItem.id !== item.id)
  await updateCart()
}

provide('addToCart', addToCart)
provide('toggleDrawer', toggleDrawer)
provide('updateCart', updateCart)
provide('removeFromCart', removeFromCart)

onBeforeMount(async () => {
  const db = await setupDB()
  await loadProducts(db).then(() => {
    productsLoaded = true
  })
  await loadCart(db)
});
</script>

<template>
  <Drawer :drawerVisible="drawerVisible">
    <Cart :cartItems="cartItems"/>
  </Drawer>
  <header class="sticky-top">
    <Header :cartItems="cartItems"/>
  </header>
  <main class="main-flex-1">
    <div class="page-banner text-center">
      <h1>Catalog de produse</h1>
      <div class="admin-panel" hidden>
        <button type="button" class="btn btn-danger me-3" @click="displayDB">Console DB</button>
        <button type="button" class="btn btn-danger" @click="killDB" >Kill DB</button>
      </div>
    </div>
    <div class="container">
      <ProductList :products="products" :productsLoaded="productsLoaded"/>
    </div>
  </main>
  <Footer/>
</template>

<style scoped>
.page-banner {
  background-color: #41B883;
  color: #ffffff;
  padding: 50px 0;
}
</style>
