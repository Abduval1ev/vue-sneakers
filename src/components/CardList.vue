<script setup>
import Card from './Card.vue';

defineProps({
    items: Array
})

</script>

<template>
    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-3 mt-4">
        <Card v-for="item in items" :key="item.id" :title="item.title" :imageUrl="item.imageUrl" :price="item.price"
            :isFavorite="item.isFavorite" />
    </div>
</template>