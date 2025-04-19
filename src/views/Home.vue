<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import Sidebar from '../components/Sidebar.vue';

// Array of notes created
const notesArray = ref([]);

// Note currently being edited
const activeNote = ref(null);

// Title and content of note
const inputTitle = ref('');
const inputContent = ref('');

const getNotes = async () => {
  try {
    const response = await axios.get('http://localhost:5000/notes');
    notesArray.value = response.data;
  } catch (error) {
    console.error('Unable to generate notes.', error);
  }
}

onMounted(async () => {
  await getNotes();
});

// Create note
const createNote = async () => {
  const id = Math.random().toString(36).substring(2,9);
  
  try {
    await axios.post('http://localhost:5000/notes', {
      id,
      title: 'Untitled',
      content: ''
    });

    await getNotes();
    setActiveNote(id);
  } catch (error) {
    console.error('Unable to create new note.', error);
  }
};

// Sets note currently being used to active
const setActiveNote = async (id) => {
  activeNote.value = id;

  let noteIndex = notesArray.value.findIndex(note => note.id === id);

  inputTitle.value = notesArray.value[noteIndex].title;
  inputContent.value = notesArray.value[noteIndex].content;

  setTimeout(() => {
    document.querySelector('input[type="text"]').focus();
  }, 0);
};

// Update note
const updateNote = () => {
  let noteIndex = notesArray.value.findIndex(note => note.id === activeNote.value);

  notesArray.value[noteIndex].title = inputTitle.value;
  notesArray.value[noteIndex].content = inputContent.value;
};

// Delete note
const deleteNote = ({id, evt}) => {
  evt.stopPropagation();

  let noteIndex = notesArray.value.findIndex(note => note.id === id);
  notesArray.value.splice(noteIndex, 1);

  activeNote.value = null;
  inputTitle.value = '';
  inputContent.value = '';
};

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