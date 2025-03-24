<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import Sidebar from './components/Sidebar.vue';

// Array of notes created
const notesArray = ref([]);

// Note currently being edited
const activeNote = ref(null);

// Title and content of note
const inputTitle = ref('');
const inputContent = ref('');

const API_URL = import.meta.env.VITE_API_URL;

onMounted(async () => {
  try {
    const response = await axios.get(API_URL);
    notesArray.value = response.data;
  } catch (error) {
    console.error("Unable to fetch notes:", error);
  }
})

// Create note
async function createNote() {
  const newNote = {
    id: Math.random().toString(36).substring(2,9),
    title: 'Untitled',
    content: ''
  };

  try {
    const { data } = await axios.post(API_URL, newNote);
    notesArray.value = [...notesArray.value, data];
  } catch (error) {
    console.error("Unable to create new note:", error);
  }
}

// Sets note currently being used to active
function setActiveNote(id) {
  activeNote.value = id;
  let note = notesArray.value.find((note) => note.id === id);

  inputTitle.value = note.title;
  inputContent.value = note.content;

  setTimeout(() => {
    document.querySelector('input[type="text"]').focus();
  }, 0);
}

// Update note
async function updateNote() {
  let note = notesArray.value.find((note) => note.id === activeNote.value);

  if (!note) return;

  try {
    await axios.put(`${API_URL}/${note.id}`, {
      ...note,
      title: inputTitle.value,
      content: inputContent.value
    });

    note.title = inputTitle.value;
    note.content = inputContent.value;
  } catch (error) {
    console.error("Unable to update note:", error);
  }
}

// Delete note
function deleteNote({id, evt}) {
  evt.stopPropagation();

  let noteIndex = notesArray.value.findIndex((note) => note.id === id);
  notesArray.value.splice(noteIndex, 1);

  activeNote.value = null;
  inputTitle = '';
  inputContent = '';
}

</script>

<template>
  <div class="bg-gradient-to-br from-yellow-50 to-yellow-200 text-gray-900 min-h-screen flex items-stretch justify-start">
    <Sidebar
      :activeNote="activeNote"
      :notesArray="notesArray"
      @new-note="createNote"
      @set-active-note="setActiveNote"
      @delete-note="deleteNote"
    />
    <main class="flex-1">
      <div v-if="activeNote"
        class="px-4 py-8 flex flex-col h-full"
        >
        <input
          v-model="inputTitle"
          @input="updateNote" 
          type="text" 
          class="block w-full text-3xl pb-2 font-bold border-b-2 border-gray-600 focus:border-yellow-200 outline-none transition-colors duration-200"
        >
        <textarea v-model="inputContent" @input="updateNote" class="block w-full h-full mt-4 text-lg outline-none flex-1"></textarea>
      </div>
    </main>
  </div>
</template>