<script setup lang="ts">
import { ref, watch, onMounted } from 'vue';
import type { UserData } from '../models/userData';
import Spinner from './Spinner.vue';
import axios from 'axios';

const isLoading = ref(false);

const props = defineProps<{
    username: string;
}>();

const userData = ref<UserData | null>(null);

const fetchUserData = async (user: string) => {
    try {
        isLoading.value = true;
        const encodedUsername = encodeURIComponent(user);
        const response = await axios.get<UserData>(`https://api.github.com/users/${encodedUsername}`);
        userData.value = response.data;
        isLoading.value = false;
    } catch (error) {
        console.error('There was a problem with the fetch operation:', error);
    }
};

onMounted(() => {
    fetchUserData(props.username);
});

watch(() => props.username, (newUsername) => {
    fetchUserData(newUsername);
});
</script>

<template>
    <div v-if="isLoading" class="mt-4 bg-[#212a46] w-2/3 2xl:w-2/5 lg:w-3/5 sm:w-2/3 py-10 rounded-xl px-5">
        <Spinner />
    </div>
    <div v-if="userData && isLoading === false" class="mt-4 bg-[#212a46] w-2/3 2xl:w-2/5 lg:w-3/5 sm:w-2/3 py-10 rounded-xl px-5">
        <div class="absolute">
            <img :src="userData.avatar_url" class="w-24 rounded-full" alt="img-profile">
        </div>
        <div class="pl-32 bg-[#212a46]">
            <div class="flex justify-between mb-5">
                <div>
                    <p class="text-2xl">
                        {{ userData.name || userData.login }}
                    </p>
                    <p class="text-sm text-[#2a5ab5]">
                        {{ userData.login }}
                    </p>
                    <p v-if="userData.bio" class="text-gray-300 mt-4 w-64 text-sm h-10 overflow-y-scroll">
                        {{ userData.bio }}
                    </p>
                    <p v-else class="text-gray-400">This profile has no bio</p>
                </div>
                <div>
                    <p class="text-gray-300 text-sm">Joined {{ new Date(userData.created_at).toLocaleDateString() }}</p>
                </div>
            </div>
            <div class="flex justify-around bg-[#161c2e] p-2 rounded-xl">
                <div>
                    <p class="text-gray-300 text-sm">Repos</p>
                    <p class="text-lg">{{ userData.public_repos }}</p>
                </div>
                <div>
                    <p class="text-gray-300 text-sm">Followers</p>
                    <p class="text-lg">{{ userData.followers }}</p>
                </div>
                <div>
                    <p class="text-gray-300 text-sm">Following</p>
                    <p class="text-lg">{{ userData.following }}</p>
                </div>
            </div>
            <div class="grid grid-cols-2 gap-6 mt-6 text-sm text-gray-300">
                <div class="flex items-center">
                    <img src="../icons/location.svg" alt="location" class="w-5">
                    <p class="p-1">{{ userData.location || 'Not Available' }}</p>
                </div>
                <div class="flex items-center">
                    <img src="../icons/twitter.svg" alt="twitter" class="w-5">
                    <p class="p-1" :class="{ 'text-gray-400': !userData.twitter_username }">
                        {{ userData.twitter_username ? `@${userData.twitter_username}` : 'Not Available' }}
                    </p>
                </div>

                <div class="flex items-center">
                    <img src="../icons/link.svg" alt="link" class="w-5">
                    <a v-if="userData.blog" class="p-1" :href="userData.blog" target="_blank">{{ userData.blog }}</a>
                    <p v-else class="p-1 text-gray-400">Not Available</p>
                </div>
                <div class="flex items-center">
                    <img src="../icons/building.svg" alt="building" class="w-5">
                    <p v-if="userData.company" class="p-1">{{ userData.company }}</p>
                    <p v-else class="p-1 text-gray-400">Not Available</p>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
::-webkit-scrollbar {
    width: 4px;
}

::-webkit-scrollbar-track {
    background-color: #161c2e;
}

::-webkit-scrollbar-thumb {
    border: 1px solid rgba(0, 0, 0, 0.384);
    background-color: #39466e;
    border-radius: 2px;
}
</style>
