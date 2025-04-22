<script setup>
import { ref } from 'vue';
import contacts from './contacts.json'
import trophy from './assets/trophy.png'

const topContacts = ref(contacts.slice(0, 5))

const addBUtton= ref(null);

const addContact = () => {
  const existingContacts = new Set(topContacts.value.map(contact => contact.id));
  const remainingContacts = contacts.filter(contact => !existingContacts.has(contact.id));
  if (remainingContacts.length === 0) {
    addBUtton.value.innerHTML = 'SORRY, NO MORE CONTACTS LEFT';
    return
  };
  const randomIndex = Math.floor(Math.random()* remainingContacts.length);
  const randomContact = remainingContacts[randomIndex];

  topContacts.value.unshift(randomContact);
}

const sortByPopularity = () => {
  topContacts.value.sort((a, b) => b.popularity - a.popularity)
}

const sortByName = () => {
  topContacts.value.sort((a, b) => a.name.localeCompare(b.name))
}

</script>

<template>
  <h1>Iron Contacts</h1>

  <div class="buttons-container">
    <button ref="addBUtton" @click="addContact">Add Random Contact</button>
    <button @click="sortByPopularity">Sort By Popularity</button>
    <button @click="sortByName">Sort By Name</button>
  </div>
 
  <table>
    <thead>
      <th>Picture</th>
      <th>Name</th>
      <th>Popularity</th>
      <th>Won Oscar</th>
      <th>Won Emmy</th>
    </thead>

    <tr v-for="contact in topContacts" :key="contact.id">
      <!-- Picture -->
      <td>
        <img class="portrait" :src="contact.pictureUrl">
      </td>

      <!-- Name -->
      <td>{{ contact.name }}</td>

      <!-- Popularity -->
      <td>{{ contact.popularity.toFixed(2) }}</td>

      <!-- Won Oscar -->
      <td v-if="contact.wonOscar === true">
        <img class="trophy" :src="trophy">
      </td>

      <!-- Won Emmy -->
      <td v-if="contact.wonEmmy === true">
        <img class="trophy" :src="trophy">
      </td>
    </tr>

    
    
  </table>
</template>



<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  width: 100%;
  min-width: 500px
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

h1 {
  text-align: center;
}
.buttons-container {
  width: 100%;
  height: auto;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
  margin: 20px auto 40px
}

button {
  text-align: center;
  min-width: 150px;
  height: 30px;
  padding: 5px
}

button:hover {
  background-color: rgb(166, 161, 161);
  border: none;
  color: white
}

table {
  margin: 0 auto;
}

.portrait {
  width: 50px;
}

th, td {
  width: 100px;
  text-align: center;
}

.trophy {
  width: 35px
}
</style>
