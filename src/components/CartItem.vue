<script setup>
defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number
})

const emit = defineEmits(['onClickRemove'])
</script>

<template>
  <div class="flex max-[370px]:flex-col items-center border border-slate-200 p-4 rounded-xl gap-4">
    <img class="w-20 h-20 max-[370px]:w-3/4 max-[370px]:h-3/4" :src="imageUrl" alt="Sneaker" />

    <div class="flex flex-col flex-1">
      <p>{{ title }}</p>
      <div class="flex justify-between mt-2">
        <b class="flex-1">{{ price }} руб.</b>
        <img
          @click="emit('onClickRemove')"
          class="opacity-40 w-8 hover:opacity-100 transition"
          src="/close.svg"
          alt="Close"
        />
      </div>
    </div>
  </div>
</template>
