<script setup>
const props = defineProps(['activeNote', 'notesArray']);
</script>

<template>
  <aside class="w-[256px] min-h-screen border-r border-gray-600">
    <div class="p-4">
      <button class="button w-full" @click="$emit('new-note')">Note +</button>
    </div>

    <ul>
      <li 
        v-for="note in props.notesArray"
        :key="note.id"
        @click="$emit('set-active-note', note.id)"
        class="p-4 border-b border-gray-600 hover:bg-yellow-300 cursor-pointer flex justify-between items-center gap-4">
        <span>{{ note.title }}</span>
        <button 
          class="text-red-500 cursor-pointer"
          @click="(evt) => $emit('delete-note', {id: note.id, evt})">Delete</button>
      </li>
    </ul>
  </aside>
</template>