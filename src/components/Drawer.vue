<script setup>
import { ref, computed, inject } from 'vue'
import DrawerHead from './DrawerHead.vue'
import CartItem from './CartItem.vue'
import InfoBlock from './InfoBlock.vue'

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number
})

const emit = defineEmits(['closeDrawer', 'addOrder'])

const { cart, removeFromCart } = inject('cart')

const isCreating = ref(false)
const orderId = ref(null)

const createOrder = () => {
  if (cart.value.length === 0) return
  
  isCreating.value = true
  
  const newOrder = {
    id: Date.now(),
    date: new Date().toLocaleString(),
    items: [...cart.value],
    totalPrice: props.totalPrice
  }
  
  emit('addOrder', newOrder)
  
  while(cart.value.length) {
    cart.value.pop()
  }
  localStorage.setItem('cart', JSON.stringify([]))
  
  orderId.value = newOrder.id
  isCreating.value = false
}

const cartIsEmpty = computed(() => cart.value.length === 0)
const buttonDisabled = computed(() => isCreating.value || cartIsEmpty.value)

const closeDrawer = () => {
  emit('closeDrawer')
}
</script>

<template>
  <!-- Затемнение на всю ширину -->
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70" @click="closeDrawer"></div>
  
  <!-- Адаптивный Drawer:
       Mobile: на всю ширину, без скруглений, отступы 16px
       md (768px): ширина 384px, скругления слева, отступы 32px -->
  <div class="bg-white h-full fixed right-0 top-0 z-20 shadow-xl overflow-y-auto transition-all"
       :class="[
         'w-full md:w-96',
         'rounded-none md:rounded-l-3xl',
         'p-4 md:p-8'
       ]">
    
    <DrawerHead @close-drawer="closeDrawer" />
    
    <div v-if="!totalPrice || orderId" class="flex h-full items-center">
      <InfoBlock
        v-if="!totalPrice && !orderId"
        title="Корзина пустая"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
        image-url="/package-icon.png"
        :show-button="false"
      />
      <InfoBlock
        v-if="orderId"
        title="Заказ оформлен!"
        :description="`Ваш заказ #${orderId} скоро будет передан курьерской доставке`"
        image-url="/order-success-icon.png"
        :show-button="true"
      />
    </div>
    
    <div v-else class="flex flex-col h-full">
      <div class="flex-1">
        <CartItem
          v-for="item in cart"
          :key="item.id"
          :id="item.id"
          :title="item.title"
          :price="item.price"
          :image-url="item.imageUrl"
          @on-click-remove="() => removeFromCart(item.id)"
        />
      </div>
      
      <div class="flex flex-col gap-3 md:gap-4 mt-5 md:mt-7 pt-4 border-t">
        <div class="flex gap-2 text-sm md:text-base">
          <span>Итого:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ totalPrice }} ₽</b>
        </div>
        <div class="flex gap-2 text-sm md:text-base">
          <span>Налог 5%:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ vatPrice }} ₽</b>
        </div>
        <button
          :disabled="buttonDisabled"
          @click="createOrder"
          class="mt-3 md:mt-4 transition bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-300 hover:bg-lime-600 active:bg-lime-700 cursor-pointer text-sm md:text-base font-medium min-h-[48px] md:min-h-auto"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>