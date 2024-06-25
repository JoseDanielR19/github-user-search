<script setup lang="ts">
import { ref } from 'vue';

const searchInput = ref('');

const emit = defineEmits<{
  (e: 'search', username: string): void;
}>();

const handleSubmit = (e: Event) => {
  e.preventDefault();
  if (searchInput.value.trim() !== '') {
    const encodedUsername = encodeURIComponent(searchInput.value.trim());
    emit('search', encodedUsername);
  }
};
</script>

<template>
  <form @submit="handleSubmit" class="w-2/3 2xl:w-2/5 lg:w-3/5 sm:w-2/3">
    <div class="flex items-center bg-[#212a46] rounded-xl overflow-hidden p-2">
      <img src="../icons/search.svg" alt="search" class="bg-[#212a46]">
      <input type="text" v-model="searchInput" class="flex-grow bg-[#212a46] text-white border-none focus:outline-none focus:ring-0 pl-2" placeholder="Search GitHub username..." />
      <button type="submit" class=" bg-[#3477f5] text-white px-4 py-2 hover:bg-[#1768ff] ease-out duration-300 rounded-xl">
        Search
      </button>
    </div>
  </form>
</template>
