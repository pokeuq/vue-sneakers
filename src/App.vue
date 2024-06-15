<script setup>
import Header from './components/Header.vue'
import Drawer from './components/Drawer.vue'
import { ref, watch, provide, computed } from 'vue'

const cart = ref([])

const drawerOpen = ref(false)

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))

const closeDrawer = () => {
  drawerOpen.value = false
}

const openDrawer = () => {
  drawerOpen.value = true
}

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true }
)

provide('cart', {
  cart,
  closeDrawer,
  openDrawer,
  addToCart,
  removeFromCart
})
</script>

<template>
  <div class="bg-white w-full lg:w-4/5 m-auto rounded-xl shadow-xl lg:mt-14 mt-0">
    <Header :total-price="totalPrice" @open-drawer="openDrawer" />
    <Drawer v-if="drawerOpen" :total-price="totalPrice" :vat-price="vatPrice" />
    <div class="p-6 sm:p-10">
      <router-view></router-view>
    </div>
  </div>
</template>

<style scoped></style>
