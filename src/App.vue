<script setup>
import { ref } from 'vue';
import contacts from './contacts.json'
import trophy from './assets/trophy.png'

const topContacts = ref(contacts.slice(0, 45))

const addButton= ref(null);

const addContact = () => {
  const existingContacts = new Set(topContacts.value.map(contact => contact.id));
  const remainingContacts = contacts.filter(contact => !existingContacts.has(contact.id));
  if (remainingContacts.length === 0) {
    addButton.value.innerHTML = 'SORRY, NO MORE CONTACTS LEFT';
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

const deleteContact = (id) => {
  topContacts.value = topContacts.value.filter((contact) => contact.id !== id);
  if (topContacts.value.length > 0) {
    addButton.value.innerHTML = "Add Random Contact";
    return
  }
}


</script>

<template>
  <h1>Iron Contacts</h1>

  <div class="buttons-container">
    <button ref="addButton" @click="addContact" class="addButton">Add Random Contact</button>
    <button @click="sortByPopularity" class="sortButton">Sort By Popularity</button>
    <button @click="sortByName" class="nameButton">Sort By Name</button>
  </div>
 
  <table>
    <thead>
      <th>Picture</th>
      <th>Name</th>
      <th>Popularity</th>
      <th>Won Oscar</th>
      <th>Won Emmy</th>
      <th>Actions</th>
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

      <td v-else>
        <p></p>
      </td>

      <!-- Won Emmy -->
      <td v-if="contact.wonEmmy === true">
        <img class="trophy" :src="trophy">
      </td>

      <td v-else>
        <p></p>
      </td>

      <!-- Actions -->
      <td>
        <button class="deleteButton" @click="deleteContact(contact.id)">Delete</button>
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


html, body {
  background: linear-gradient(90deg, #e3ffe7 0%, #d9e7ff 100%);
  width: 100%;
  min-width: 450px;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

h1 {
  text-align: center;
  
}

h1, th, td {
  color: #1A2980
}

.buttons-container {
  width: auto;
  height: auto;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
  margin: 20px auto 40px
}

.addButton, .sortButton, .nameButton {
  background-image: linear-gradient(to right, #1A2980 0%, #26D0CE  51%, #1A2980  100%);
  text-align: center;
  min-width: 140px;
  height: 30px;
  padding: 5px;
  border-radius: 12px;
  border: none;
  transition: 0.5s;
  background-size: 200% auto;
  color: white;            
  box-shadow: 0 0 20px #eee;
  border-radius: 10px;
  display: block;
}

.addButton:hover, .sortButton:hover, .nameButton:hover {
  background-position: right center; 
  color: #fff;
  text-decoration: none;

}

.deleteButton {
  text-align: center;
  min-width: 100px;
  height: 30px;
  padding: 5px;
  border-radius: 50px;
  border: none;
  background-image: linear-gradient(to right, #e52d27 0%, #b31217  51%, #e52d27  100%);
  transition: 0.5s;
  background-size: 200% auto;
  color: white;            
  box-shadow: 0 0 20px #eee;
  display: block;
}

.deleteButton:hover {
  background-position: right center; /* change the direction of the change here */
  color: #fff;
  text-decoration: none;
}         

table {
  width: auto;
  margin: 0 auto;
}

.portrait {
  width: 50px;
  border-radius: 5px
}

th, td {
  width: 100px;
  text-align: center;
}

.trophy {
  width: 35px
}
</style>
