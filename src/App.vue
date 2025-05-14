<script setup>
import { computed, onMounted, provide, reactive, ref, watch } from "vue";
import axios from "axios";

import Header from "./components/Header.vue";
import CardList from "./components/CardList.vue";
import Drawer from "./components/Drawer.vue";

const items = ref([]);
const cartItems = ref([]);
const drawerOpen = ref(false);
const totalPrice = computed(() => cartItems.value.reduce((acc, item) => acc + item.price, 0));

const closeDriver = () => {
  drawerOpen.value = false;
};
const openDriver = () => {
  drawerOpen.value = true;
};

const filters = reactive({
  sortBy: "title",
  searchQuery: "",
});

const onchangeSelect = (event) => {
  filters.sortBy = event.target.value;
};

const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value;
};

const addToCart = async (item) => {
  item.isAdded = true;
  cartItems.value.push(item);
};
const removeFromCart = async (item) => {
  item.isAdded = false;
  cartItems.value.slice(cartItems.value.indexOf(item), 1);
};

const onClickAddPlus = async (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(
      `https://aa335fb96eba5e2c.mokky.dev/favorites`
    );
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id);

      if (!favorite) {
        return item;
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id,
      };
    });
  } catch (error) {
    console.error("Error fetching items:", error);
  }
};

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id,
      };
      item.isFavorite = true;

      const { data } = await axios.post(
        `https://aa335fb96eba5e2c.mokky.dev/favorites`,
        obj
      );

      item.favoriteId = data.id;

    } else {
      item.isFavorite = false;
      await axios.delete(
        `https://aa335fb96eba5e2c.mokky.dev/favorites/${item.favoriteId}`
      );
      item.favoriteId = null;
    }
  } catch (error) { }
};

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
    };

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`;
    }

    const { data } = await axios.get(`https://aa335fb96eba5e2c.mokky.dev/items`, {
      params,
    });
    items.value = data.map((obj) => ({
      ...obj,
      isAdded: false,
      favoriteId: null,
      isFavorite: false,
    }));
  } catch (error) {
    console.error("Error fetching items:", error);
  }
};

onMounted(async () => {
  await fetchItems();
  await fetchFavorites();
});
watch(filters, fetchItems);
provide("cart", { cartItems, closeDriver, openDriver, addToCart, removeFromCart, onClickAddPlus });
</script>

<template>
  <main class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Drawer v-if="drawerOpen" />
    <Header :total-price="totalPrice" @openDriver="openDriver" />

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
            <img src="/search.svg" class="absolute left-3 top-3" alt="Search" />
            <input type="text" @input="onChangeSearchInput"
              class="border border-gray-200 rounded-md py-2 pl-10 pr-4 outline-none focus:border-gray-400 duration-300"
              placeholder="Poisk..." />
          </div>
        </div>
      </div>
      <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus" />
    </div>
  </main>
</template>
