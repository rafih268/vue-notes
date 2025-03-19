<script setup>
import { ref } from 'vue';
import Sidebar from './components/Sidebar.vue';

// Array of notes created
const notesArray = ref([]);

// Note currently being edited
const activeNote = ref(null);

// Title and content of note
const inputTitle = ref('');
const inputContent = ref('');

// Creates a note
function createNote() {
  const id = Math.random().toString(36).substring(2,9);
  notesArray.value.push({
    id,
    title: 'Untitled',
    content: ''
  });
}

// Sets note currently being used to be active
function setActiveNote(id) {
  activeNote.value = id;
  let note = notesArray.value.find((note) => note.id === id);

  inputTitle.value = note.title;
  inputContent.value = note.content;

  setTimeout(() => {
    document.querySelector('input[type="text"]').focus();
  }, 0);
}

</script>

<template>
  <div class="bg-gradient-to-br from-yellow-50 to-yellow-200 text-gray-900 min-h-screen flex items-stretch justify-start">
    <Sidebar
      :activeNote="activeNote"
      :notesArray="notesArray"
    />
    <main class="flex-1">
      <div v-if="activeNote"
        class="px-4 py-8 flex flex-col h-full"
        >
        <input
          v-model="inputTitle" 
          type="text" 
          class="block w-full text-3xl pb-2 font-bold border-b-2 border-gray-600 focus:border-yellow-200 outline-none transition-colors duration-200"
        >
        <textarea v-model="inputContent" class="block w-full h-full mt-4 text-lg outline-none flex-1"></textarea>
      </div>
    </main>
  </div>
</template>