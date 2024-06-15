<script setup>
defineProps({
  totalPrice: Number
})

const emit = defineEmits(['openDrawer'])
</script>

<template>
  <header
    class="flex flex-col gap-6 items-center sm:gap-0 sm:flex-row sm:justify-between border-b border-slate-200 px-10 py-6"
  >
    <router-link to="/">
      <div class="flex items-center gap-6">
        <img src="/logo.png" alt="Logo" class="w-20 sm:w-10" />
        <div>
          <h2 class="font-bold text-xl max-[500px]:text-base uppercase">Vue Sneakers</h2>
          <p class="text-slate-400 max-[500px]:text-sm">Магазин лучших кроссовок</p>
        </div>
      </div>
    </router-link>

    <ul class="flex items-center gap-8 transition font-medium">
      <router-link to="/favorites">
        <li class="flex items-center gap-3 cursor-pointer text-gray-500 hover:text-black">
          <img src="/heart.svg" alt="Heart" class="w-7" />
          <span>Закладки</span>
        </li>
      </router-link>
      <li
        @click="() => emit('openDrawer')"
        class="flex items-center gap-3 cursor-pointer text-gray-500 hover:text-black"
      >
        <img src="/cart.svg" alt="Cart" class="w-7" />
        <b>{{ totalPrice }} руб.</b>
      </li>
    </ul>
  </header>
</template>
