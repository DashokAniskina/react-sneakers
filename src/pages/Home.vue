<script setup>
import { ref, onMounted, watch, reactive } from 'vue'
import debounce from 'lodash.debounce'
import Card from '../components/Card.vue'

// Данные о кроссовках (12 штук)
const sneakersList = [
  { id: 1, title: "Мужские Кроссовки Nike Blazer Mid Suede", price: 12999, imageUrl: "sneakers-1.jpg" },
  { id: 2, title: "Мужские Кроссовки Nike Air Max 270", price: 12999, imageUrl: "sneakers-2.jpg" },
  { id: 3, title: "Мужские Кроссовки Nike Blazer Mid Suede", price: 8499, imageUrl: "sneakers-3.jpg" },
  { id: 4, title: "Кроссовки Puma X Aka Boku Future Rider", price: 8999, imageUrl: "sneakers-4.jpg" },
  { id: 5, title: "Мужские Кроссовки Jordan Air Jordan 11", price: 10799, imageUrl: "sneakers-5.jpg" },
  { id: 6, title: "Мужские Кроссовки Nike LeBron XVIII", price: 16499, imageUrl: "sneakers-6.jpg" },
  { id: 7, title: "Мужские Кроссовки Under Armour Curry 8", price: 15199, imageUrl: "sneakers-7.jpg" },
  { id: 8, title: "Мужские Кроссовки Nike Kyrie 7", price: 11299, imageUrl: "sneakers-8.jpg" },
  { id: 9, title: "Мужские Кроссовки Nike Lebron XVIII Low", price: 13999, imageUrl: "sneakers-9.jpg" },
  { id: 10, title: "Мужские Кроссовки Nike Kyrie Flytrap IV", price: 11299, imageUrl: "sneakers-10.jpg" },
  { id: 11, title: "Мужские Кроссовки Adidas Ultraboost", price: 15999, imageUrl: "sneakers-11.jpg" },
  { id: 12, title: "Мужские Кроссовки Puma RS-X", price: 9999, imageUrl: "sneakers-12.jpg" }
]

const items = ref([])
const cart = ref([])
const favorites = ref([])

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

// Загрузка из localStorage
const loadCart = () => {
  const savedCart = localStorage.getItem('cart')
  if (savedCart) {
    cart.value = JSON.parse(savedCart)
  }
}

const loadFavorites = () => {
  const savedFavorites = localStorage.getItem('favorites')
  if (savedFavorites) {
    favorites.value = JSON.parse(savedFavorites)
  }
}

// Добавление в корзину
const addToCart = (itemId) => {
  console.log('Добавление в корзину ID:', itemId)
  
  const item = items.value.find(i => i.id === itemId)
  if (!item) {
    console.log('Товар не найден')
    return
  }
  
  if (!item.isAdded) {
    // Добавляем в корзину
    const cartItem = {
      id: item.id,
      title: item.title,
      price: item.price,
      imageUrl: item.imageUrl,
      isAdded: true
    }
    cart.value.push(cartItem)
    item.isAdded = true
    console.log('Товар добавлен в корзину, корзина:', cart.value.length)
  } else {
    // Удаляем из корзины
    const index = cart.value.findIndex(cartItem => cartItem.id === itemId)
    if (index !== -1) {
      cart.value.splice(index, 1)
      item.isAdded = false
      console.log('Товар удален из корзины')
    }
  }
  
  localStorage.setItem('cart', JSON.stringify(cart.value))
}

// Добавление в избранное
const addToFavorite = (itemId) => {
  console.log('Добавление в избранное ID:', itemId)
  
  const item = items.value.find(i => i.id === itemId)
  if (!item) {
    console.log('Товар не найден')
    return
  }
  
  if (!item.isFavorite) {
    favorites.value.push(itemId)
    item.isFavorite = true
    console.log('Товар добавлен в избранное, избранное:', favorites.value.length)
  } else {
    const index = favorites.value.findIndex(favId => favId === itemId)
    if (index !== -1) {
      favorites.value.splice(index, 1)
      item.isFavorite = false
      console.log('Товар удален из избранного')
    }
  }
  
  localStorage.setItem('favorites', JSON.stringify(favorites.value))
}

// Фильтрация и сортировка
const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
  updateItemsDisplay()
}

const onChangeSearchInput = debounce((event) => {
  filters.searchQuery = event.target.value
  updateItemsDisplay()
}, 300)

const updateItemsDisplay = () => {
  let filtered = [...sneakersList]
  
  if (filters.searchQuery) {
    filtered = filtered.filter(item => 
      item.title.toLowerCase().includes(filters.searchQuery.toLowerCase())
    )
  }
  
  if (filters.sortBy === 'price') {
    filtered.sort((a, b) => a.price - b.price)
  } else if (filters.sortBy === '-price') {
    filtered.sort((a, b) => b.price - a.price)
  } else if (filters.sortBy === 'title') {
    filtered.sort((a, b) => a.title.localeCompare(b.title))
  }
  
  items.value = filtered.map(item => ({
    ...item,
    isFavorite: favorites.value.includes(item.id),
    isAdded: cart.value.some(cartItem => cartItem.id === item.id)
  }))
}

// Инициализация
onMounted(() => {
  loadCart()
  loadFavorites()
  updateItemsDisplay()
})

// Следим за изменениями
watch(cart, () => {
  localStorage.setItem('cart', JSON.stringify(cart.value))
  updateItemsDisplay()
}, { deep: true })

watch(favorites, () => {
  localStorage.setItem('favorites', JSON.stringify(favorites.value))
  updateItemsDisplay()
}, { deep: true })
</script>

<template>
  <div class="p-10">
    <!-- Баннер -->
    <div class="relative mb-12 rounded-2xl overflow-hidden">
      <img src="/sneakers/banner.png" alt="Stan Smith Forever" class="w-full h-auto object-cover" />
    </div>

    <!-- Фильтры -->
    <div class="flex justify-between items-center mb-8 flex-wrap gap-4">
      <h2 class="text-3xl font-bold">Все кроссовки</h2>
      <div class="flex gap-4">
        <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
          <option value="title">По названию</option>
          <option value="price">По цене (дешевые)</option>
          <option value="-price">По цене (дорогие)</option>
        </select>
        <div class="relative">
          <img class="absolute left-4 top-3 w-4 h-4" src="/search.svg" alt="Search" />
          <input
            @input="onChangeSearchInput"
            class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400 w-64"
            type="text"
            placeholder="Поиск..."
          />
        </div>
      </div>
    </div>

    <!-- Сетка карточек -->
    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
      <Card
        v-for="item in items"
        :key="item.id"
        :id="item.id"
        :title="item.title"
        :image-url="item.imageUrl"
        :price="item.price"
        :is-favorite="item.isFavorite"
        :is-added="item.isAdded"
        :on-favorite-click="addToFavorite"
        :on-add-click="addToCart"
      />
    </div>
    
    <p v-if="items.length === 0" class="text-center text-gray-500 py-10">Товары не найдены</p>
  </div>
</template>