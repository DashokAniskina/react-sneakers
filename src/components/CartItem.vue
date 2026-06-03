<script setup>
defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number
})

const emit = defineEmits(['onClickRemove'])

const onClickRemove = () => {
  emit('onClickRemove')
}
</script>

<template>
  <div class="flex items-center border border-slate-200 p-3 md:p-4 rounded-xl gap-3 md:gap-4 mb-3">
    <img class="w-12 h-12 md:w-14 md:h-14 lg:w-16 lg:h-16 object-contain" :src="`/sneakers/${imageUrl}`" :alt="title" />
    <div class="flex flex-col flex-1">
      <p class="text-xs md:text-sm">{{ title }}</p>
      <div class="flex justify-between mt-1 md:mt-2">
        <b class="flex-1 text-sm md:text-base">{{ price.toLocaleString() }} руб.</b>
        <img
          @click="onClickRemove"
          class="w-4 h-4 md:w-auto md:h-auto opacity-40 hover:opacity-100 cursor-pointer transition min-w-[20px] min-h-[20px]"
          src="/close.svg"
          alt="Remove"
        />
      </div>
    </div>
  </div>
</template>