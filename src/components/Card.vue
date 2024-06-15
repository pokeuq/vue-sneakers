<script setup>
defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickFavorite: Function,
  onClickAdd: Function
})
</script>

<template>
  <div
    class="relative flex flex-col bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer hover:-translate-y-2 hover:shadow-xl transition"
  >
    <img
      v-if="onClickFavorite"
      :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
      alt="Like"
      class="absolute top-8 left-8 h-10 w-10"
      @click="onClickFavorite"
    />
    <img class="object-contain" :src="imageUrl" alt="Sneaker" />
    <div class="flex flex-col justify-stretch mt-auto">
      <p class="mt-4">{{ title }}</p>
      <div class="flex justify-between items-center mt-2">
        <div class="flex flex-col">
          <span class="text-slate-400">Цена: </span>
          <b>{{ price }} руб.</b>
        </div>
        <img
          v-if="onClickAdd"
          :src="!isAdded ? '/plus.svg' : '/checked.svg'"
          alt="Plus"
          @click="onClickAdd"
          class="h-10 w-10"
        />
      </div>
    </div>
  </div>
</template>
