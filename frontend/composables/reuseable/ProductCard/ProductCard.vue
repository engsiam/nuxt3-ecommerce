<template>
  <div class="relative">
    <div
      class="relative flex items-center justify-center p-12 bg-gray-100 cursor-pointer group"
    >
      <!-- Discount Badge -->
      <div
        v-if="product.discount"
        class="absolute px-2 py-1 text-xs font-bold rounded-md text-light bg-primary top-2 left-2"
      >
        {{ product.discount }}
      </div>
      <div>
        <NuxtLink to="/product/{id}">
          <img
            :src="product.image"
            alt="Product Image"
            class="object-contain w-full h-40"
          />
        </NuxtLink>
      </div>

      <div class="absolute flex flex-col space-y-2 right-4 top-2">
        <button
          class="p-2 text-black bg-white rounded-full"
          @click="addToWishlist"
        >
          <Icon
            :icon="whishList ? 'mdi:heart' : 'mdi:heart-outline'"
            :class="['text-xl', { 'text-primary': whishList }]"
          />
        </button>
        <button class="p-2 text-black bg-white rounded-full">
          <Icon icon="mdi:eye-outline" class="text-xl" />
        </button>
      </div>

      <!-- Add to Cart button on hover -->
      <div
        class="absolute bottom-0 left-0 right-0 flex items-center justify-center text-white transition-opacity duration-300 bg-black opacity-0 bg-opacity-90 group-hover:opacity-100"
      >
        <button class="w-full py-2" @click="handleAddToCart">Add To Cart</button>
      </div>
    </div>

    <NuxtLink to="/product/{id}">
      <div class="mt-4 text-left">
        <h3 class="text-lg font-semibold">{{ product.name }}</h3>
        <div class="flex items-center gap-4 mt-2">
          <div class="text-primary">{{ product.price }}</div>
          <div class="line-through text-secondary">{{ product.prevPrice }}</div>
        </div>
        <div class="flex items-center mt-2">
          <Rating :rating="product.rating" />
          <span class="ml-2 text-sm text-gray-500"
            >({{ product.ratingCount }})</span
          >
        </div>
      </div>
    </NuxtLink>
  </div>
</template>

<script setup>
import { Icon } from "@iconify/vue";
import Rating from "~/composables/reuseable/Rating/Rating.vue";

const whishList = ref(false);

const props = defineProps({
  product: {
    type: Object,
    required: true,
  },
});
const addToWishlist = () => {
  whishList.value = !whishList.value;
};
const cartItems = ref([])
const handleAddToCart = () => {
  cartItems.value.push(props.product);
};
</script>

<style>
.word {
  color: red;
}
</style>
