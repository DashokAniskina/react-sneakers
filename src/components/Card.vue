<script setup>
const props = defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onFavoriteClick: Function,
  onAddClick: Function
})

const handleFavorite = () => {
  if (props.onFavoriteClick) {
    props.onFavoriteClick(props.id)
  }
}

const handleAdd = () => {
  if (props.onAddClick) {
    props.onAddClick(props.id)
  }
}
</script>

<template>
  <div class="relative bg-white border border-slate-100 rounded-2xl md:rounded-3xl p-3 md:p-4 lg:p-5 cursor-pointer transition hover:-translate-y-2 hover:shadow-xl">
    <img
      :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
      alt="Like"
      class="absolute top-2 left-2 md:top-4 md:left-4 lg:top-5 lg:left-5 w-6 h-6 md:w-7 md:h-7 lg:w-8 lg:h-8 z-10 cursor-pointer hover:scale-110 transition"
      @click.stop="handleFavorite"
    />
    
    <div class="flex justify-center h-32 sm:h-36 md:h-40 lg:h-48">
      <img :src="`/sneakers/${imageUrl}`" :alt="title" class="object-contain h-full" />
    </div>
    
    <p class="mt-2 md:mt-3 font-medium text-sm md:text-base">{{ title }}</p>
    
    <div class="flex justify-between items-center mt-2 md:mt-3 lg:mt-4">
      <div class="flex flex-col">
        <span class="text-slate-400 text-xs md:text-sm">Цена:</span>
        <b class="text-base md:text-lg">{{ price.toLocaleString() }} руб.</b>
      </div>
      
      <img
        @click.stop="handleAdd"
        :src="!isAdded ? '/plus.svg' : '/checked.svg'"
        alt="Add to cart"
        class="w-7 h-7 md:w-8 md:h-8 cursor-pointer hover:scale-110 transition"
      />
    </div>
  </div>
</template>