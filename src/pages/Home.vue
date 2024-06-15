<script setup>
import CardList from '../components/CardList.vue'
import axios from 'axios'
import { inject, ref, reactive, watch, onMounted } from 'vue'
const { cart, addToCart, removeFromCart } = inject('cart')

const items = ref([])
const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}
const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        item_id: item.id
      }

      item.isFavorite = true

      const { data } = await axios.post(
        `https://0071c7edacfa1086.mokky.dev/favorites
`,
        obj
      )

      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(`https://0071c7edacfa1086.mokky.dev/favorites
/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (err) {
    console.log(err)
  }
}

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearch = (event) => {
  filters.searchQuery = event.target.value
}
const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://0071c7edacfa1086.mokky.dev/favorites')
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.item_id === item.id)
      if (!favorite) {
        return item
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (err) {
    console.log(err)
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }
    const { data } = await axios.get('https://0071c7edacfa1086.mokky.dev/items', { params })
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }))
  } catch (err) {
    console.log(err)
  }
}

onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cart.value = localCart ? JSON.parse(localCart) : []

  await fetchItems()
  await fetchFavorites()

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
  }))
})

watch(filters, fetchItems)

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})
</script>

<template>
  <div class="flex flex-col lg:flex-row lg:justify-between lg:items-center mb-8 gap-5">
    <h2 class="text-3xl font-bold">Все кроссовки</h2>

    <div class="flex flex-col sm:flex-row gap-4">
      <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
        <option value="name">По названию</option>
        <option value="price">По цене (дешевые)</option>
        <option value="-price">По цене (дорогие)</option>
      </select>

      <div class="relative">
        <img class="absolute left-4 top-3.5" src="/search.svg" alt="Search" />
        <input
          @input="onChangeSearch"
          class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400 transition w-full"
          placeholder="Поиск..."
        />
      </div>
    </div>
  </div>

  <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus" />
</template>
