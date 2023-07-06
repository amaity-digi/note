<script setup>
import { ref } from 'vue';
const isModalOpen = ref(false);
const newNote = ref('');
const errorMessage = ref('');
const notes = ref([]);

function randomColor() {
  return 'hsl(' + (Math.random() * 360) + ', 100%, 70%)';
}

const addNotes = () => {
  if (newNote.value.length < 5) {
    return errorMessage.value = 'Must Should enter atleast 5 Characters!!!';
  }

  notes.value.push({
    id: Math.floor(Math.random() * 10000),
    text: newNote.value,
    date: new Date(),
    background: randomColor()
  });
  newNote.value = '';
  isModalOpen.value = false;
  errorMessage.value = '';
}


const deleteNote = (noteId) => {
  const index = notes.value.findIndex((note) => note.id === noteId); //It searches for the first element that satisfies the provided testing function
  console.log(index, 'iiii');
  if (index !== -1) {
    notes.value.splice(index, 1);
  }
};

const editNote = (noteId) => {
  const noteIndex = notes.value.findIndex((note) => note.id === noteId);
  console.log(noteIndex, 'index');
  if (noteIndex !== -1) {
    newNote.value = notes.value[noteIndex].text;
    isModalOpen.value = true;
    deleteNote(noteId);
  }
};

</script>

<template>
  <main>
    <div v-if="isModalOpen" class="overlay">
      <div class="modal">
        {{ JSON.stringify(notes, null, 2) }}
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p class="error" v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNotes">Add Note</button>
        <button class="close" @click="isModalOpen = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="isModalOpen = true">+</button>
      </header>
      <div class="cards-container">
        <div v-for=" note in notes" :key="note.id" :style="{ backgroundColor: note.background }" class="card">
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString() }}</p>
          <button class="update" @click="editNote(note.id)">Update</button>
          <button class="delete" @click="deleteNote(note.id)">Delete</button>
        </div>

      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 90vw
}

.container {
  max-width: 800px;
  padding: 10px;
  margin: 0 auto
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}

header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(21, 20, 20);
  border-radius: 100%;
  color: white;
  font-size: 20px;
}

.card {
  width: 225px;
  height: 225px;
  background-color: hwb(59 10% 4%);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.date {
  font-size: 12.5px;
  font-weight: bold;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: absolute;
  width: 98vw;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  width: 750px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: #2b6be2;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 15px
}

.modal .close {
  background-color: rgb(193, 15, 15);
  margin-top: 7px;
}

.error {
  color: brown;
}

.delete {
  border: none;
  padding: 10px;
  width: 80px;
  height: 40px;
  cursor: pointer;
  background-color: rgb(21, 20, 20);
  border-radius: 10%;
  color: white;
  font-size: 15px;
}

.update {
  padding: 10px;
  width: 80px;
  height: 40px;
  cursor: pointer;
  background-color: rgb(21, 20, 20);
  border-radius: 10%;
  color: white;
  font-size: 15px;
}
</style>