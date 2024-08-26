<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea
          v-model.trim="newNote"
          name="note"
          id="note"
          cols="30"
          rows="10"
          lang="it"
        ></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote" class="add-note-button">Add Note</button>
        <button @click="showModal = false" class="close-button">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true" class="add-button" title="Add a note">
          +
        </button>
      </header>
      <div class="cards-container">
        <p v-if="showEmptyMessage" class="empty-content-text">
          <i>There are no notes at the moment. Click the button to add one </i
          >😊
        </p>
        <div
          v-for="note in notes"
          :key="note.id"
          class="card"
          :style="{ backgroundColor: note.backgroundColor }"
        >
          <p class="main-text">{{ note.text }}</p>
          <div class="date-delete-contaier">
            <p class="date">{{ note.date.toLocaleDateString("it-EU") }}</p>
            <button @click="deleteNote(note.id)" class="delete-bouton">
              x
            </button>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script setup>
import { ref } from "vue";

const showModal = ref(false);
const newNote = ref("");
const errorMessage = ref("");
const notes = ref([]);
const showEmptyMessage = ref(true);

function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

const addNote = () => {
  if (newNote.value.length < 7) {
    return (errorMessage.value = "Note needs to be 7 characters or more");
  }
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor(),
  });

  showModal.value = false;
  newNote.value = "";
  errorMessage.value = "";
  showEmptyMessage.value = false;
};

const deleteNote = (id) => {
  notes.value = notes.value.filter((note) => note.id !== id);
  if (notes.value.length === 0) {
    showEmptyMessage.value = true;
  }
};
</script>

<style scoped>
.empty-content-text {
  font-size: 15px;
  margin-top: 20px;
  position: absolute;
}
.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}

header {
  margin-top: 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 60px;
}

.add-button {
  border: none;
  padding: 10px;
  width: 30px;
  height: 30px;
  cursor: pointer;
  background-color: black;
  border-radius: 100%;
  color: white;
  font-size: 10px;
}

.card {
  width: 225px;
  height: 225px;
  background-color: pink;
  padding: 15px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
  transition: 0.2s ease-in-out;
}

.card:hover {
  box-shadow: -8px 1px 34px -15px rgba(0, 0, 0, 0.75);
}

.date {
  font-size: 13px;
  font-weight: bold;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.date-delete-contaier {
  display: flex;
  justify-content: space-between;
}

.delete-bouton {
  background-color: black;
  color: white;
  border-radius: 100%;
  width: 20px;
}

.delete-bouton:hover {
  cursor: pointer;
  opacity: 0.8;
}

.overlay {
  position: absolute;
  width: 100%;
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

#note {
  resize: vertical;
  padding: 10px;
}
.add-note-button {
  padding: 10px 20px;
  font-size: 20px;
  border: none;
  cursor: pointer;
  background-color: black;
  color: white;
  margin-bottom: 10px;
  margin-top: 10px;
}

.close-button {
  padding: 10px 20px;
  font-size: 20px;
  border: none;
  cursor: pointer;
  background-color: rgb(245, 110, 245);
  color: white;
}

.modal p {
  color: red;
  margin-top: 5px;
}
</style>
