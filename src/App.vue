<script setup lang="ts">
import { ref } from 'vue';
import InputSearch from "./components/InputSearch.vue";
import CardUser from "./components/CardUser.vue";

const currentUsername = ref('josedanielr19');

const handleSearch = (username: string) => {
  currentUsername.value = username;
};
</script>

<template>
  <div class="text-white font-mono grid content-center justify-items-center w-full h-screen bg-[#161c2e]">
    <InputSearch @search="handleSearch" />
    <CardUser :username="currentUsername" />
  </div>
</template>
