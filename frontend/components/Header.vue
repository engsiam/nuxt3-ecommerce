<template>
  <div>
    <!-- Top header -->
    <div class="hidden px-4 py-2 text-white bg-black md:flex lg:px-6">
      <div class="container flex items-center justify-between mx-auto">
        <div class="flex-1 text-center">
          <span class="text-sm">
            Summer Sale For All Swim Suits And Free Express Delivery - OFF 50%!
          </span>
          <a href="#" class="hover:underline">
            <span class="ml-4 font-semibold text-white">ShopNow</span>
          </a>
        </div>
        <div class="flex">
          <select
            class="px-2 py-1 ml-4 text-white bg-transparent border-0 rounded-md hover:text-black hover:bg-gray-100 focus:outline-none"
          >
            <option value="en">English</option>
            <option value="fr">Français</option>
            <!-- Add more languages as needed -->
          </select>
        </div>
      </div>
    </div>
    <!-- Main header -->
    <header class="bg-white border-b">
      <div
        class="container flex items-center justify-between px-4 py-4 mx-auto lg:px-6"
      >
        <!-- Logo -->
        <div class="flex items-center">
          <NuxtLink to="/">
            <span class="text-xl font-bold">Exclusive</span>
          </NuxtLink>
        </div>
        <!-- Navigation Links for larger screens -->
        <nav class="hidden space-x-10 lg:flex">
          <NuxtLink
            v-for="menu in menus"
            :key="menu.text"
            :href="menu.link"
            class="font-bold text-black text-md hover:underline"
            >{{ menu.text }}</NuxtLink
          >
        </nav>
        <!-- Icons and Menu button -->
        <div class="relative flex items-center justify-center space-x-2">
          <!-- Search icon and search box for smaller screens -->
          <div class="lg:hidden">
            <div>
              <button class="text-gray-600 hover:text-black focus:outline-none">
                <Icon
                  @click="search"
                  name="material-symbols:search"
                  class="w-6 h-6"
                />
              </button>
            </div>
            <div
              class="absolute inset-0 z-20 flex items-center justify-center top-20 -left-36"
              v-if="isSearch"
            >
              <div
                class="flex items-center px-4 py-2 space-x-2 bg-white border rounded-md"
              >
                <input
                  type="text"
                  placeholder="What are you looking for?"
                  class="text-black bg-[#F5F5F5] focus:outline-none"
                />
                <button
                  @click="search"
                  class="text-gray-600 hover:text-black focus:outline-none"
                >
                  <Icon name="material-symbols:close" class="w-6 h-6" />
                </button>
              </div>
            </div>
          </div>
          <!-- Icons mobile -->
          <NuxtLink
            v-for="icon in icons"
            :key="icon.name"
            :to="icon.link"
            class="text-gray-600 lg:hidden hover:text-black focus:outline-none"
          >
            <Icon
              :name="icon.name"
              class="flex items-center justify-center w-6 h-6"
            />
          </NuxtLink>
          <div
            class="relative text-gray-600 bloc lg:hidden hover:text-black focus:outline-none"
          >
            <Icon
              name="mdi:cart-outline"
              class="relative w-6 h-6 cursor-pointer"
              @click="toggleSidebar"
            />
            <div
              class="absolute flex items-center justify-center w-1 h-1 p-3 text-xs font-bold text-white rounded-full -right-2 -top-2 bg-primary"
            >
              20
            </div>
          </div>

          <button
            @click="toggleMenu"
            class="text-gray-600 lg:hidden hover:text-black focus:outline-none"
          >
            <Icon name="material-symbols:menu" class="w-6 h-6" />
          </button>
          <!-- Search bar -->
          <div class="relative hidden lg:block w-60">
            <input
              type="text"
              placeholder="What are you looking for?"
              class="w-full text-black px-4 py-2 border rounded-md bg-[#F5F5F5] focus:outline-none"
            />
            <button
              class="absolute text-gray-600 transform -translate-y-1/2 right-2 top-1/2"
            >
              <Icon name="material-symbols:search" class="w-5 h-5" />
            </button>
          </div>
          <!-- Icons -->
          <NuxtLink
            v-for="icon in icons"
            :key="icon.name"
            :to="icon.link"
            class="hidden text-gray-600 lg:block hover:text-black focus:outline-none"
          >
            <Icon :name="icon.name" class="w-6 h-6" />
          </NuxtLink>
          <!-- cart icon -->
          <div
            class="relative hidden text-gray-600 bloc lg:block hover:text-black focus:outline-none"
          >
            <Icon
              name="mdi:cart-outline"
              class="relative w-6 h-6 cursor-pointer"
              @click="toggleSidebar"
            />
            <div
              class="absolute flex items-center justify-center w-1 h-1 p-3 text-xs font-bold text-white rounded-full -right-2 -top-2 bg-primary"
            >
              20
            </div>
          </div>
          <Sidebar
            v-model:visible="visibleRight"
            header="Your Cart"
            position="right"
            class="w-[22rem]"
          >
            <Cart />
          </Sidebar>
          <!-- add to cart end -->
        </div>
      </div>
      <!-- Sidebar menu for mobile and tablet -->
      <transition name="slide">
        <div v-if="isMenuOpen" class="fixed inset-y-0 z-50 flex">
          <div
            class="flex-1 h-full bg-black opacity-50"
            @click="toggleMenu"
          ></div>
          <div
            class="flex flex-col h-full p-6 space-y-4 overflow-y-auto bg-white shadow-md opacity-95 w-80"
          >
            <button
              @click="toggleMenu"
              class="flex items-center mb-6 text-gray-600 hover:text-black focus:outline-none"
            >
              <Icon name="material-symbols:close" class="w-6 h-6" />
            </button>
            <NuxtLink to="/">
              <span class="text-xl font-bold">Exclusive</span>
            </NuxtLink>
            <Category />
          </div>
        </div>
      </transition>
    </header>
  </div>
</template>

<script setup>
import Sidebar from "primevue/sidebar";
import { ref } from "vue";

import "primeicons/primeicons.css";
import "primevue/resources/primevue.min.css";
import "primevue/resources/themes/saga-blue/theme.css";

const visibleRight = ref(false);

const toggleSidebar = () => {
  visibleRight.value = !visibleRight.value;
};

//import Icon from "nuxtjs/icon";

const isMenuOpen = ref(false);
const isSearch = ref(false);
const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value;
};
const search = () => {
  isSearch.value = !isSearch.value;
};

const menus = [
  { text: "Home", link: "/" },
  { text: "Contact", link: "/contact" },
  { text: "About", link: "/about" },
  { text: "Sign Up", link: "/signup" },
];

const icons = [
  {
    name: "mdi:heart-outline",
    link: "/wishlist",
  },

  {
    name: "mdi:account",
    link: "",
  },
];
</script>

<style scoped>
.slide-enter-active,
.slide-leave-active {
  transition: transform 0.3s ease;
}
.slide-enter,
.slide-leave-to {
  transform: translateX(-100%);
}
</style>

<style>
.p-sidebar-content {
  padding: 0;
}
</style>
