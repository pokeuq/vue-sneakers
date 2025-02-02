<script setup>
import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'
import InfoBlock from './InfoBlock.vue'
import axios from 'axios'
import { ref, watch, provide, computed, inject } from 'vue'

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number,
  cartButtonDisabled: Boolean
})

const { cart, closeDrawer } = inject('cart')

const isCreating = ref(false)
const orderId = ref(null)

const createOrder = async () => {
  try {
    isCreating.value = true
    const { data } = await axios.post('https://0071c7edacfa1086.mokky.dev/orders', {
      items: cart.value,
      totalPrice: props.totalPrice.value
    })

    cart.value = []

    orderId.value = data.id
  } catch (e) {
    console.log(e)
  } finally {
    isCreating.value = false
  }
}

const buttonDisabled = computed(() => isCreating.value || cartIsEmpty.value)
const cartIsEmpty = computed(() => cart.value.length === 0)
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div
    class="bg-white w-[600px] max-[640px]:w-full h-full fixed right-0 top-0 z-20 p-8 overflow-y-auto scrollbar-none"
  >
    <DrawerHead />

    <div v-if="!totalPrice || orderId" class="flex h-full items-center">
      <InfoBlock
        v-if="!totalPrice && !orderId"
        title="Корзина пустая"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
        image-url="/package-icon.png"
      />
      <InfoBlock
        v-if="orderId"
        title="Заказ оформлен!"
        :description="`Ваш заказ #${orderId} скоро будет передан курьерской доставке`"
        image-url="/order-success-icon.png"
      />
    </div>

    <CartItemList />

    <div v-if="totalPrice" class="flex flex-col gap-3 mt-7">
      <div class="flex gap-2 items-end">
        <span>Итого:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ totalPrice }}₽</b>
      </div>

      <div class="flex gap-2 items-end">
        <span>Налог 5%:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ vatPrice }}₽</b>
      </div>
      <button
        :disabled="buttonDisabled"
        @click="createOrder"
        class="mt-4 bg-lime-500 w-full rounded-xl py-3 text-white hover:bg-lime-600 active:bg-lime-700 disabled:bg-slate-300 cursor-pointer transition"
      >
        Оформить заказ
      </button>
    </div>
  </div>
</template>

<style scoped>
.scrollbar-none::-webkit-scrollbar {
  display: none;
}
.scrollbar-none {
  -ms-overflow-style: none; /* IE and Edge */
  scrollbar-width: none; /* Firefox */
}
</style>
>
