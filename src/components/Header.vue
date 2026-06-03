<script setup>
defineProps({
  totalPrice: Number,
  openDrawer: Function
})
</script>

<template>
  <header class="flex flex-col sm:flex-row justify-between items-center border-b border-slate-200 px-4 md:px-6 lg:px-10 py-4 md:py-6 lg:py-8 gap-4 sm:gap-0">
    <!-- Логотип -->
    <div class="flex items-center gap-2 md:gap-3 lg:gap-4 cursor-pointer" @click="$router.push('/')">
      <img src="/logo.png" alt="Logo" class="w-8 md:w-9 lg:w-10" />
      <div>
        <h2 class="text-base md:text-lg lg:text-xl font-bold uppercase">REACT SNEAKER</h2>
        <p class="text-slate-400 text-xs md:text-sm hidden sm:block">Магазин лучших кроссовок</p>
      </div>
    </div>
    
    <!-- Навигация -->
    <ul class="flex items-center gap-4 md:gap-6 lg:gap-10">
      <!-- Корзина -->
      <li @click="openDrawer" class="flex items-center cursor-pointer gap-2 md:gap-3 text-gray-500 hover:text-black min-h-[44px]">
        <img src="/cart.svg" alt="Cart" class="w-5 h-5 md:w-auto md:h-auto" />
        <b class="text-sm md:text-base">{{ totalPrice }} руб.</b>
      </li>
      
      <!-- Закладки -->
      <li @click="$router.push('/favorites')" class="flex items-center cursor-pointer gap-2 md:gap-3 text-gray-500 hover:text-black min-h-[44px]">
        <img src="/heart.svg" alt="Favorites" class="w-5 h-5 md:w-auto md:h-auto" />
        <span class="text-sm md:text-base hidden sm:inline">Закладки</span>
      </li>
      
      <!-- Профиль -->
      <li @click="$router.push('/profile')" class="flex items-center cursor-pointer gap-2 md:gap-3 text-gray-500 hover:text-black min-h-[44px]">
        <img src="/profile.svg" alt="Profile" class="w-5 h-5 md:w-auto md:h-auto" />
        <span class="text-sm md:text-base hidden sm:inline">Профиль</span>
      </li>
    </ul>
  </header>
</template>