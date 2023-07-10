<script setup>
import { ref } from "vue"; // Importing the 'ref' function from the Vue.js library

function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

// Creating reactive variables using the 'ref' function
const showModal = ref(false); // Represents whether a modal should be shown or not
const newNote = ref(""); // Represents the value of a new note input
const notes = ref([]); // Represents an array of notes
const errormessage = ref(""); // Represents an error message

// Function to add a new note
const addnote = () => {
  if (newNote.value.length < 10) {
    return (errormessage.value = "the letter is less than 10 characters");
  }

  // Pushing a new note object to the 'notes' array
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor(),
  });

  showModal.value = false; // Hiding the modal
  newNote.value = ""; // Clearing the new note input
  errormessage.value = ""; // Clearing the error message
}
</script>


<template>
 <main>
 <div v-if="showModal"  class="overlay">
      <div class="modal">
        <textarea name="note" id="note" cols="30" rows="10" v-model.trim="newNote"></textarea>
        <p v-if="errormessage">{{ errormessage }}</p>
        <button @click="addnote">Add Note</button>
        <button class="close" @click="showModal = false">close</button>
        </div></div>
  <div class="container">
    <header>
      <h1>Notes </h1>
      <button @click="showModal = true">+</button>
      </header>

      <div class="card-container">
        <div v-for="note in notes" :key="note.id" class="card" :style="{backgroundColor:note.backgroundColor}">
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString("en-US") }}</p>
          </div>
        </div>
    </div>
  </main>
</template>

<style scoped>
body{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

main{
  width: 100vw;
  height: 100vh;
}
.container{
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}
header{
  display: flex;
  justify-content: space-between;
  align-items: center;
}
h1{
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}
header button{
  background-color: black;
  border: none;
  padding: 10px;
  cursor: pointer;
  height: 50px;
  width: 50px;
  border-radius: 100%;
  color: white;
  font-size: 24px;
}
.card{
  width: 225px;
  height: 225px;
  background-color: pink;
  padding: 10px;
  border-radius: 15px;
  flex-direction: column;
  display: flex;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}
.date{
  font-size: 12.5px;
  font-weight: bold;
}
.card-container{
  display: flex;
  flex-wrap: wrap;
}
.overlay{
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(71, 71, 71, 0.568);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}


.modal{
  width: 550px;
  background-color: #fff;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}
.modal button{
  padding: 10px 20px ;
  font-size: 20px;
  width: 100%;
  background-color: blue;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 15px;
}
.modal .close{
  background-color: red;
  margin-top: 7px;
}
textarea {
  resize: none;
}
</style>
