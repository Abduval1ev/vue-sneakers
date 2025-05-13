<script setup>
import DrawerHead from "./DrawerHead.vue"
import CartListItem from "./CartListItem.vue"
</script>

<template>
    <div class="fixed inset-0 h-full w-full bg-black opacity-70 z-10"></div>
    <div class="bg-white w-[70%] h-full fixed right-0 top-0 z-20 p-8">
        <DrawerHead />

        <CartListItem />

        <div class="flex flex-col gap-4 mt-auto">
            <div class="flex gap-2">
                <span>Itogo:</span>
                <div class="flex-1 border-b border-dashed"></div>
                <b>1299 P</b>
            </div>
            <div class="flex gap-2">
                <span>Nalog 5%:</span>
                <div class="flex-1 border-b border-dashed"></div>
                <b>900 P</b>
            </div>
            <button disabled
                class="mt-2 bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-300 hover:bg-lime-600 active:bg-lime-700 cursor-pointer duration-300">Oformit
                zakaz</button>
        </div>
    </div>
</template>f