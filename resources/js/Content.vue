<template>
  <div class="p-5 mb-5 border border-gray-300 rounded-lg">
    <h3 class="mb-4 text-lg font-semibold">User Information</h3>
    <div class="mb-4">
      <label for="name" class="block mb-1 font-bold text-gray-700">Name:</label>
      <input
        type="text"
        id="name"
        v-model="formData.name"
        class="w-full p-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
      />
    </div>
    <div class="mb-4">
      <label for="email" class="block mb-1 font-bold text-gray-700">Email:</label>
      <input
        type="email"
        id="email"
        v-model="formData.email"
        class="w-full p-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
      />
    </div>
    <div class="mb-0">
      <label for="message" class="block mb-1 font-bold text-gray-700">Message:</label>
      <textarea
        id="message"
        v-model="formData.message"
        rows="4"
        class="w-full p-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
      ></textarea>
    </div>
  </div>
</template>

<script setup>
import { reactive } from 'vue';

const formData = reactive({
  name: '',
  email: '',
  message: ''
});

// Expose formData for parent component to access
defineExpose({
  formData
});
</script>
