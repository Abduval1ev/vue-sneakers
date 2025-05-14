<script setup>
defineProps({
    id: Number,
    title: String,
    price: Number,
    imageUrl: String,
    isAdded: Boolean,
    onClickAdd: Function,
})

const emit = defineEmits(['onClickRemove'])
</script>


<template>
    <div class="flex items-center border border-slate-200 p-4 rounded-xl gap-2">
        <img class="w-16 h-16" :src="imageUrl" :alt="title">

        <div class="flex flex-col justify-between flex-1">
            <p>{{ title }}</p>
            <div class="flex justify-between items-center">
                <b class="flex-1">{{ price }} p</b>
                <img @click="emit('onClickRemove')" class="cursor-pointer opacity-40 hover:opacity-100 duration-300"
                    src="/close.svg" alt="Close">
            </div>
        </div>
    </div>
</template>