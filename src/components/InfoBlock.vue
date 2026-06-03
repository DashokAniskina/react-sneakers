<script setup>
defineProps({
  title: String,
  description: String,
  imageUrl: String,
  showButton: Boolean
})
</script>

<template>
  <div class="flex flex-col items-center text-center w-72 mx-auto">
    <img height="80" width="80" :src="imageUrl" alt="Info image" />
    <h2 class="mt-4 text-2xl font-medium">{{ title }}</h2>
    <p class="text-gray-400 mt-2">{{ description }}</p>
    <button 
      v-if="showButton" 
      @click="$router.push('/')"
      class="mt-6 bg-lime-500 text-white px-6 py-2 rounded-full hover:bg-lime-600 transition"
    >
      Вернуться назад
    </button>
  </div>
</template>