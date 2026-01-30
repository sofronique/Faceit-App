<script setup>
import {ref} from 'vue'
const playerNick = ref('')
let playerObject = ref({})
const players = ref([])

playerObject = {
    name: '',
    country: '',
    created: ''
}

async function onSubmit(){
const apiKey = 'b188a03d-3f2a-4af9-bee7-8f576041f40a';
const url = 'https://open.faceit.com/data/v4/search/players?nickname=';

fetch(url + playerNick.value, {
  method: 'GET',
  headers: {
    'accept': 'application/json',
    'Authorization': `Bearer ${apiKey}`
  }
}).then(response => response.json())
  .then(data => {
    players.value = data.items
    console.log(players.value)

    if (playerObject) {playerObject = {
    name: '',
    country: '',
    created: ''
}}

    playerObject.value = data
    playerObject.country = data.country
    playerObject.name = data.nickname
    playerObject.created = data.activated_at?.slice(0, 10)

    playerNick.value = ''
  })}
</script>

<template>
    <h1>Search for a user</h1>
    <input v-model="playerNick"/>
    <input type="button" @click="onSubmit()" value="Search">

    <div v-if="players">
        <ul>
            <li v-for="player in players"> {{ player.nickname + ' ' + player.country}} </li>
        </ul>
    </div>

    <div v-if="playerObject">
        <h2>{{playerObject.name}}</h2>
        <h2>{{playerObject.country}}</h2>
        <h2>{{playerObject.created}}</h2>
    </div>
</template>

<style scoped>
ul {
    list-style-type: none;
}
</style>
