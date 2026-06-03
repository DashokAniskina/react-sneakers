<script setup>
import { ref, computed, provide, onMounted } from 'vue'
import Drawer from './components/Drawer.vue'
import Header from './components/Header.vue'

const cart = ref([])
const drawerOpen = ref(false)
const orders = ref([])

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))

const closeDrawer = () => {
  drawerOpen.value = false
}

const openDrawer = () => {
  drawerOpen.value = true
}

const removeFromCart = (itemId) => {
  const index = cart.value.findIndex(cartItem => cartItem.id === itemId)
  if (index !== -1) {
    cart.value.splice(index, 1)
    localStorage.setItem('cart', JSON.stringify(cart.value))
  }
}

const addOrder = (order) => {
  orders.value.push(order)
  localStorage.setItem('orders', JSON.stringify(orders.value))
}

onMounted(() => {
  const savedCart = localStorage.getItem('cart')
  if (savedCart) {
    cart.value = JSON.parse(savedCart)
  }
  
  const savedOrders = localStorage.getItem('orders')
  if (savedOrders) {
    orders.value = JSON.parse(savedOrders)
  }
})

provide('cart', {
  cart,
  removeFromCart
})
</script>

<template>
  <Drawer 
    v-if="drawerOpen" 
    :total-price="totalPrice" 
    :vat-price="vatPrice" 
    @close-drawer="closeDrawer"
    @add-order="addOrder"
  />

  <div class="bg-white w-full md:w-11/12 lg:w-10/12 xl:w-4/5 m-auto rounded-none md:rounded-xl shadow-none md:shadow-xl mt-0 md:mt-7 lg:mt-10 xl:mt-14">
    <Header :total-price="totalPrice" :open-drawer="openDrawer" />
    <router-view />
  </div>
</template>