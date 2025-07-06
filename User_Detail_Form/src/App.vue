<template>
  <div class="p-6 max-w-4xl mx-auto">
    <h1 class="text-3xl font-bold mb-6 text-center">Our Team</h1>

    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-6">
      <div v-for="user in users" :key="user.id" class="bg-white p-4 shadow rounded-lg text-center">
        <img :src="user.image" alt="Profile" class="w-24 h-24 rounded-full mx-auto mb-4 object-cover" />
        <h2 class="text-lg font-semibold">{{ user.firstName }} {{ user.lastName }}</h2>
        <a :href="'mailto:' + user.email" class="text-blue-600 hover:underline text-sm">
          Contact
        </a>
      </div>
    </div>

    <!-- Pagination -->
    <div class="flex justify-center mt-6 space-x-2">
      <button
        v-for="page in totalPages"
        :key="page"
        @click="currentPage = page"
        :class="[
          'px-3 py-1 rounded',
          currentPage === page ? 'bg-blue-500 text-white' : 'bg-gray-200 text-gray-800'
        ]"
      >
        {{ page }}
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, watchEffect } from 'vue'

const users = ref([])
const currentPage = ref(1)
const totalPages = 3 // Based on dummyjson limit=6 and total users = 30

const fetchUsers = async () => {
  const limit = 6
  const skip = (currentPage.value - 1) * limit
  const res = await fetch(`https://dummyjson.com/users?limit=${limit}&skip=${skip}`)
  const data = await res.json()
  users.value = data.users
}

watchEffect(fetchUsers)
</script>

<style>
body {
  @apply bg-gray-50;
}
</style>