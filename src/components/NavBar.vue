<!--
md: 기준으로 반응형 처리함. 
-->
<template>
  <!--- PC뷰 -->
  <nav class="hidden md:flex justify-between p-6 px-4">
    <div class="flex justify-between items-center w-full">
      <!-- 로고 -->
      <div class="md:w-1/3">
        <router-link class="block max-w-max" to="/">
          <img :src="logoSrc" alt="logo" />
        </router-link>
      </div>
      <!-- 네비메뉴 -->
      <div class="md:block md:w-1/3">
        <ul class="flex justify-center">
          <li
            class="mr-12 hover:text-violet-500"
            v-for="item in navItems"
            :key="item.text"
          >
            <a
              :href="item.link"
              class="text-coolGray-500 hover:text-coolGray-900 font-medium"
            >
              {{ item.text }}
            </a>
          </li>
        </ul>
      </div>
      <!-- 로그인 -->
      <div class="md:block md:w-1/3">
        <div class="flex items-center justify-end">
          <div v-if="!user" class="flex items-center gap-4">
            <router-link
              class="inline-block py-2 px-4 text-sm leading-5 text-violet-50 bg-violet-500 hover:bg-violet-600 font-medium focus:ring-2 focus:ring-violet-500 focus:ring-opacity-50 rounded-md"
              to="/login"
              >Login</router-link
            >
            <router-link
              class="border border-grey-500 inline-block py-2 px-4 text-sm leading-5 font-medium focus:ring-2 focus:ring-violet-500 focus:ring-opacity-50 rounded-md"
              to="/signup"
              >Sign Up</router-link
            >
          </div>
          <div v-if="user" class="flex items-center gap-4">
            <span>Hello, {{ user.email }} !</span>
            <!-- <router-link
              class="inline-block py-2 px-4 text-sm leading-5 text-violet-50 bg-violet-500 hover:bg-violet-600 font-medium focus:ring-2 focus:ring-violet-500 focus:ring-opacity-50 rounded-md"
              to="/notes"
              >dashboard</router-link
            > -->
            <button
              class="inline-block py-2 px-4 text-sm leading-5 text-white bg-violet-500 hover:bg-violet-600 font-medium text-center focus:ring-2 focus:ring-violet-500 focus:ring-opacity-50 rounded-md"
              @click="logout"
            >
              Logout
            </button>
          </div>
        </div>
      </div>
    </div>
  </nav>

  <!-- 모바일뷰 -->
  <!-- 햄버거바 버튼 -->
  <button @click="isMenuOpen = !isMenuOpen" class="md:hidden p-4">
    <font-awesome-icon
      icon="fa-bars"
      class="text-gray-500"
      style="font-size: 35; font-weight: bold"
    />
  </button>

  <!-- 모바일 네비바 -->
  <div
    class="navbar-menu fixed top-0 left-0 z-50 w-full h-full bg-gray-900 bg-opacity-50"
    :class="{ 'translate-x-0': isMenuOpen, '-translate-x-full': !isMenuOpen }"
    style="transition: 0.5s"
  >
    <div
      class="fixed top-0 left-0 bottom-0 w-full w-4/6 max-w-xs bg-white transition-transform duration-300 ease-in-out"
    >
      <nav class="relative p-6 h-full overflow-y-auto">
        <div class="flex flex-col justify-between h-full">
          <a class="inline-block" href="#">
            <img :src="logoSrc" alt="logo" />
          </a>
          <ul class="py-6 h-full flex flex-col justify-start">
            <li
              class="hover:text-violet-500"
              v-for="item in navItems"
              :key="item.text"
            >
              <a
                :href="item.link"
                class="block py-3 px-4 text-coolGray-500 hover:text-coolGray-900 text-lg font-medium hover:bg-coolGray-50 rounded-md"
              >
                {{ item.text }}
              </a>
            </li>
          </ul>
          <div class="flex flex-wrap">
            <div class="w-full mb-2">
              <router-link
                class="mb-2 text-white bg-violet-500 inline-block py-2 px-4 w-full text-sm leading-5 text-coolGray-500 hover:text-coolGray-900 bg-transparent font-medium text-center rounded-md"
                v-if="!user"
                to="/login"
                >Login</router-link
              >
            </div>
            <div class="w-full">
              <router-link
                class="border border-grey-500 inline-block py-2 px-4 w-full text-sm leading-5 font-medium text-center focus:ring-2 focus:ring-violet-500 focus:ring-opacity-50 rounded-md"
                v-if="!user"
                to="/signup"
                >Sign Up</router-link
              >
            </div>
            <span
              class="mb-2 inline-block py-2 px-4 w-full text-sm leading-5 text-coolGray-500 hover:text-coolGray-900 bg-transparent font-medium text-center rounded-md"
              v-if="user"
              >Hello, {{ user.email }}</span
            >
            <!-- <div class="w-full mb-2">
              <router-link
                class="inline-block py-2 px-4 w-full text-sm leading-5 text-white bg-violet-500 hover:bg-violet-600 font-medium text-center focus:ring-2 focus:ring-violet-500 focus:ring-opacity-50 rounded-md"
                v-if="user"
                to="/notes"
                >dashboard</router-link
              >
            </div> -->
            <div class="w-full mb-2">
              <a
                class="inline-block py-2 px-4 w-full text-sm leading-5 text-white bg-violet-500 hover:bg-violet-600 font-medium text-center focus:ring-2 focus:ring-violet-500 focus:ring-opacity-50 rounded-md"
                v-if="user"
                @click="logout"
              >
                LogOut
              </a>
            </div>
          </div>
        </div>
      </nav>
      <button @click="isMenuOpen = false" class="absolute top-5 right-3 p-4">
        <svg
          width="12"
          height="12"
          viewBox="0 0 12 12"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M6.94004 6L11.14 1.80667C11.2656 1.68113 11.3361 1.51087 11.3361 1.33333C11.3361 1.1558 11.2656 0.985537 11.14 0.860002C11.0145 0.734466 10.8442 0.66394 10.6667 0.66394C10.4892 0.66394 10.3189 0.734466 10.1934 0.860002L6.00004 5.06L1.80671 0.860002C1.68117 0.734466 1.51091 0.663941 1.33337 0.663941C1.15584 0.663941 0.985576 0.734466 0.860041 0.860002C0.734505 0.985537 0.66398 1.1558 0.66398 1.33333C0.66398 1.51087 0.734505 1.68113 0.860041 1.80667L5.06004 6L0.860041 10.1933C0.797555 10.2553 0.747959 10.329 0.714113 10.4103C0.680267 10.4915 0.662842 10.5787 0.662842 10.6667C0.662842 10.7547 0.680267 10.8418 0.714113 10.9231C0.747959 11.0043 0.797555 11.078 0.860041 11.14C0.922016 11.2025 0.99575 11.2521 1.07699 11.2859C1.15823 11.3198 1.24537 11.3372 1.33337 11.3372C1.42138 11.3372 1.50852 11.3198 1.58976 11.2859C1.671 11.2521 1.74473 11.2025 1.80671 11.14L6.00004 6.94L10.1934 11.14C10.2554 11.2025 10.3291 11.2521 10.4103 11.2859C10.4916 11.3198 10.5787 11.3372 10.6667 11.3372C10.7547 11.3372 10.8419 11.3198 10.9231 11.2859C11.0043 11.2521 11.0781 11.2025 11.14 11.14C11.2025 11.078 11.2521 11.0043 11.286 10.9231C11.3198 10.8418 11.3372 10.7547 11.3372 10.6667C11.3372 10.5787 11.3198 10.4915 11.286 10.4103C11.2521 10.329 11.2025 10.2553 11.14 10.1933L6.94004 6Z"
            fill="#556987"
          />
        </svg>
      </button>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import { supabase } from "../../util/supabase/supabase";
import { logout } from "../../util/supabase/authUtils";
import { getSessionData } from "../../util/supabase/authUtils";

// 로고 경로 관리
const logoSrc = ref("/urlachive-logo.png");
// 네비바 메뉴는 여기서 관리
const navItems = ref([
  // { text: "Product", link: "#" },
  // { text: "Features", link: "#" },
  // { text: "Pricing", link: "#" },
  // { text: "Resources", link: "#" },
]);

const isMenuOpen = ref(false);
const user = ref(null);

onMounted(async () => {
  user.value = await getSessionData();
});
</script>

<style scoped lang="scss"></style>
