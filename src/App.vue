<script setup>
import { onMounted, provide, reactive, ref, watch } from 'vue'
import axios from 'axios'

import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
import Driwer from './components/Drawer.vue'

const items = ref([])
const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearch = (event) => {
  filters.searchQuery = event.target.value
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get('https://749eed7119156be4.mokky.dev/items', {
      params
    })

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      isAdded: false
    }))
  } catch (error) {
    console.log(error)
  }
}

const getFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://749eed7119156be4.mokky.dev/favorites')
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)

      if (!favorite) {
        return item
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (error) {
    console.log(error)
  }
}

const addToFavorite = async (item) => {
  item.isFavorite = true
}

onMounted(async () => {
  await fetchItems()
  await getFavorites()
})

watch(filters, fetchItems)

provide('addToFavorite', addToFavorite)
</script>

<template>
  <div>
    <!-- <Driwer /> -->
    <div class="w-4/5 mx-auto bg-white rounded-xl shadow-xl mt-14">
      <Header />
      <div class="p-10">
        <div class="flex justify-between items-center">
          <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>
          <div class="flex gap-4">
            <select @change="onChangeSelect" class="py-2 px-3 border rounded-xl outline-none">
              <option value="name">По названию</option>
              <option value="price">По цене(дешевые)</option>
              <option value="-price">По цене(дорогие)</option>
            </select>
            <div class="relative">
              <img class="absolute left-3 top-3" src="/search.svg" alt="" />
              <input
                @change="onChangeSearch"
                value=""
                class="border rounded-xl py-2 pl-11 pr-4 outline-none focus:border-gray-400"
                type="text"
                placeholder="Поиск..."
              />
            </div>
          </div>
        </div>
        <div class="mt-10">
          <CardList :items="items" />
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
