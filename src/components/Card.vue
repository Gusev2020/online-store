<script setup>
import { inject } from 'vue'

defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickFavorite: Function
})

const addToFavorite = inject('addToFavorite')

const onClickFavorite = () => {
  const obj = {
    parentId: props.id
  }
  addToFavorite(obj)
}
</script>
<template>
  <div
    class="relative bg-white border border-slate-200 rounded-3xl p-8 cursor-pointer hover:-translate-y-2 hover:shadow-xl transition"
  >
    <img
      class="absolute top-7 left-7"
      :src="isFavorite ? '/like-2.svg' : '/like-1.svg'"
      alt="Like"
      @click="onClickFavorite"
    />
    <img :src="imageUrl" alt="Sneaker" />
    <p class="mt-2">{{ title }}</p>

    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-400">Цена:</span>
        <b>{{ price }} руб.</b>
      </div>

      <img @click="onClickAdd" :src="isAdded ? '/checked.svg' : '/plus.svg'" alt="Plus" />
    </div>
  </div>
</template>
