<script setup>
import { inject } from 'vue';
import CartItem from './CartItem.vue';

const { cartItems, removeFromCart } = inject('cart')
</script>

<template>
    <div class="flex flex-col gap-1 sm:gap-2">
        <CartItem v-for="item in cartItems" :key="item.id" :title="item.title" :price="item.price"
            :imageUrl="item.imageUrl" @on-click-remove="() => removeFromCart(item)" />
    </div>
</template>