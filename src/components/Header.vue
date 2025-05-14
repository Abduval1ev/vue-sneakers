<script setup>
defineProps({
    totalPrice: Number
})

const emit = defineEmits(['openDriver'])
</script>

<template>
    <header class="flex  justify-between border-b border-slate-200 px-10 py-8">
        <div class="flex justify-between items-center gap-4">
            <img src="/logo.png" alt="Logo" class="w-10 cursor-pointer">
            <div>
                <h2 class="text-xl font-bold uppercase cursor-pointer">Vue Sneakers</h2>
                <p class="text-slate-400">The market best of sneakers</p>
            </div>
        </div>
        <ul class="flex items-center gap-10">
            <li @click="() => emit('openDriver')"
                class="flex items-center gap-3 text-gray-400 cursor-pointer hover:text-black duration-300">
                <img src="/cart.svg" alt="Cart">
                <b>{{ totalPrice }} P</b>
            </li>
            <li class="flex items-center gap-3 text-gray-400 cursor-pointer hover:text-black duration-300">
                <img src="/heart.svg" alt="Heart">
                Zakladka
            </li>
            <li class="flex items-center gap-3 text-gray-400 cursor-pointer hover:text-black duration-300">
                <img src="/profile.svg" alt="Profile">
                Profile
            </li>
        </ul>
    </header>
</template>