<script setup>
import { onMounted, reactive, ref, watch } from "vue";
import axios from "axios";

import Header from "./components/Header.vue";
import CardList from "./components/CardList.vue";
import Drawer from "./components/Drawer.vue";

const items = ref([]);

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const onchangeSelect = event => {
  filters.sortBy = event.target.value;
}

const onChangeSearchInput = event => {
  filters.searchQuery = event.target.value
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`https://aa335fb96eba5e2c.mokky.dev/favorites`);
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)

      if (!favorite) {
        return item
      }

      console.log(favorite, 'favo');
      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id,
      }
    });

  } catch (error) {
    console.error("Error fetching items:", error);
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

    const { data } = await axios.get(`https://aa335fb96eba5e2c.mokky.dev/items`, {
      params
    });
    items.value = data.map(obj => ({
      ...obj,
      isAdded: false,
      isFavorite: false
    }));
    console.log(data);
  } catch (error) {
    console.error("Error fetching items:", error);
  }
}

onMounted(async () => {
  await fetchItems()
  await fetchFavorites()
});
watch(filters, fetchItems)

</script>

<template>

  <main class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <!-- <Drawer /> -->
    <Header />

    <div class="p-10">
      <div class="flex justify-between">
        <h2 class="text-3xl font-bold">Vse tovari</h2>
        <div class="grid grid-cols-2 gap-4 md:grid-cols-2">
          <select @change="onchangeSelect"
            class="py-2 px-3 border border-gray-200 rounded-md outline-none focus:border-gray-400 duration-300">
            <option value="name">po nazvani</option>
            <option value="price">po chene (deshovo)</option>
            <option value="-price">po chene (doroje)</option>
          </select>
          <div class="relative">
            <img src="/search.svg" class="absolute left-3 top-3" alt="Search">
            <input type="text" @input="onChangeSearchInput"
              class="border border-gray-200 rounded-md py-2 pl-10 pr-4 outline-none focus:border-gray-400 duration-300"
              placeholder="Poisk...">
          </div>
        </div>

      </div>
      <CardList :items="items" />
    </div>
  </main>

</template>