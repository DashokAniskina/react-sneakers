<script setup>
import { ref, onMounted } from 'vue'

const orders = ref([])

onMounted(() => {
  const savedOrders = localStorage.getItem('orders')
  if (savedOrders) {
    orders.value = JSON.parse(savedOrders)
  }
})
</script>

<template>
  <div class="p-10">
    <h2 class="text-3xl font-bold mb-8">Мои покупки</h2>
    
    <div v-if="orders.length === 0" class="text-center py-10">
      <img src="/package-icon.png" alt="No orders" class="w-24 h-24 mx-auto mb-4 opacity-50" />
      <h3 class="text-xl font-medium text-gray-600">У вас нет заказов</h3>
      <p class="text-gray-400 mt-2">Вы нищеброд? Оформите хотя бы один заказ.</p>
      <router-link to="/" class="inline-block mt-6 bg-lime-500 text-white px-6 py-2 rounded-full hover:bg-lime-600 transition">
        ← Вернуться назад
      </router-link>
    </div>
    
    <div v-else class="space-y-6">
      <div v-for="order in orders.slice().reverse()" :key="order.id" class="border rounded-xl p-6 bg-gray-50">
        <div class="flex justify-between items-center mb-4 pb-3 border-b">
          <div>
            <span class="text-sm text-gray-500">Заказ #{{ order.id }}</span>
            <p class="text-sm text-gray-400">{{ order.date }}</p>
          </div>
          <div class="text-right">
            <span class="text-sm text-gray-500">Сумма заказа:</span>
            <p class="text-xl font-bold text-lime-600">{{ order.totalPrice.toLocaleString() }} ₽</p>
          </div>
        </div>
        
        <div class="space-y-3">
          <div v-for="item in order.items" :key="item.id" class="flex items-center gap-4 p-3 bg-white rounded-lg">
            <img :src="`/sneakers/${item.imageUrl}`" :alt="item.title" class="w-16 h-16 object-contain" />
            <div class="flex-1">
              <p class="font-medium">{{ item.title }}</p>
              <p class="text-gray-500 text-sm">{{ item.price.toLocaleString() }} руб.</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>