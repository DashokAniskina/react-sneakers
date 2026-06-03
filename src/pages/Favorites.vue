<script setup>
import { ref, onMounted } from 'vue'
import Card from '../components/Card.vue'

const favorites = ref([])
const cart = ref([])

// Загрузка избранного
const loadFavorites = () => {
  const savedFavorites = localStorage.getItem('favorites')
  if (savedFavorites) {
    const favIds = JSON.parse(savedFavorites)
    
    // Все кроссовки
    const allSneakers = [
      { id: 1, title: "Мужские Кроссовки Nike Blazer Mid Suede", price: 12999, imageUrl: "sneakers-1.jpg" },
      { id: 2, title: "Мужские Кроссовки Nike Air Max 270", price: 12999, imageUrl: "sneakers-2.jpg" },
      { id: 3, title: "Мужские Кроссовки Nike Blazer Mid Suede", price: 8499, imageUrl: "sneakers-3.jpg" },
      { id: 4, title: "Кроссовки Puma X Aka Boku Future Rider", price: 8999, imageUrl: "sneakers-4.jpg" },
      { id: 5, title: "Мужские Кроссовки Jordan Air Jordan 11", price: 10799, imageUrl: "sneakers-5.jpg" },
      { id: 6, title: "Мужские Кроссовки Nike LeBron XVIII", price: 16499, imageUrl: "sneakers-6.jpg" },
      { id: 7, title: "Мужские Кроссовки Under Armour Curry 8", price: 15199, imageUrl: "sneakers-7.jpg" },
      { id: 8, title: "Мужские Кроссовки Nike Kyrie 7", price: 11299, imageUrl: "sneakers-8.jpg" },
      { id: 9, title: "Мужские Кроссовки Nike Lebron XVIII Low", price: 13999, imageUrl: "sneakers-9.jpg" },
      { id: 10, title: "Мужские Кроссовки Nike Kyrie Flytrap IV", price: 11299, imageUrl: "sneakers-10.jpg" }
    ]
    
    favorites.value = allSneakers.filter(item => favIds.includes(item.id))
    favorites.value.forEach(fav => fav.isFavorite = true)
    favorites.value.forEach(fav => fav.isAdded = false)
  }
}

// Удаление из избранного
const removeFromFavorites = (itemId) => {
  const index = favorites.value.findIndex(fav => fav.id === itemId)
  if (index !== -1) {
    favorites.value.splice(index, 1)
    const savedFavorites = localStorage.getItem('favorites')
    if (savedFavorites) {
      let favIds = JSON.parse(savedFavorites)
      favIds = favIds.filter(id => id !== itemId)
      localStorage.setItem('favorites', JSON.stringify(favIds))
    }
  }
}

onMounted(() => {
  loadFavorites()
})
</script>

<template>
  <div class="p-10">
    <h2 class="text-3xl font-bold mb-8">Мои закладки</h2>
    
    <div v-if="favorites.length === 0" class="text-center py-10">
      <img src="/heart.svg" alt="No favorites" class="w-24 h-24 mx-auto mb-4 opacity-50" />
      <h3 class="text-xl font-medium text-gray-600">Закладок нет :(</h3>
      <p class="text-gray-400 mt-2">Вы ничего не добавляли в закладки</p>
      <router-link to="/" class="inline-block mt-6 bg-lime-500 text-white px-6 py-2 rounded-full hover:bg-lime-600 transition">
        ← Вернуться назад
      </router-link>
    </div>
    
    <div v-else class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
      <Card
        v-for="item in favorites"
        :key="item.id"
        :id="item.id"
        :title="item.title"
        :image-url="item.imageUrl"
        :price="item.price"
        :is-favorite="true"
        :is-added="false"
        :on-favorite-click="removeFromFavorites"
        :on-add-click="null"
      />
    </div>
  </div>
</template>