<script setup>
import DriwerHead from './DriwerHead.vue'
import CartItemList from './CartItemList.vue'
</script>
<template>
  <div>
    <div class="fixed top-0 left-0 h-full w-full z-10 bg-black opacity-70"></div>
    <div class="bg-white fixed h-full p-8 w-96 right-0 top-0 z-20">
      <DriwerHead />
      <CartItemList />

      <div class="flex flex-col gap-5 my-7">
        <div class="flex gap-2">
          <span>Итого:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>12 900 руб.</b>
        </div>
        <div class="flex gap-2">
          <span>Налог 5%:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>900 руб.</b>
        </div>
        <button
          class="bg-lime-500 w-full rounded-xl py-3 text-white hover:bg-lime-600 transition active:bg-lime-700 disabled:bg-slate-400 cursor-pointer"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>
