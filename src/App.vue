<script setup>
import Sidebar from './components/Sidebar.vue'

</script>

<template>
  <div class="bg-gradient-to-br from-yellow-50 to-yellow-200 text-gray-900 min-h-screen flex items-stretch justify-start">
    <Sidebar/>
    <main class="flex-1">
      <div class="px-4 py-8 flex flex-col h-full">
        <input type="text" class="block w-full text-3xl pb-2 font-bold border-b-2 border-gray-600 focus:border-yellow-200 outline-none transition-colors duration-200">
        <textarea class="block w-full h-full mt-4 text-lg outline-none flex-1"></textarea>
      </div>
    </main>
  </div>
</template>