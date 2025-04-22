<script setup>
import { ref } from 'vue';
import contacts from './contacts.json'

const topContacts = ref(contacts.slice(0,5))
</script>

<template>
  <h1>Iron Contacts</h1>
  <table>
    <thead>
      <th>Picture</th>
      <th>Name</th>
      <th>Popularity</th>
    </thead>

    <tr v-for="contact in topContacts" :key="contact.id">
      <td>
        <img :src="contact.pictureUrl">
      </td>

      <td>{{ contact.name }}</td>

      <td>{{ contact.popularity.toFixed(2) }}</td>
    </tr>
    
    
  </table>
</template>



<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

img {
  width: 50px;
}
</style>
