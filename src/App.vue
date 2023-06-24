<script setup>
import { onMounted } from 'vue';
import {ref} from 'vue';
import ThemeToggle from './components/ThemeToggle.vue';
import LoadingScreen from './components/LoadingScreen.vue';
import { Waypoint } from 'vue-waypoint'


const pokeList = ref([])
var isLoading = true
var limit = 5


async function fetchKantoPokemon() {
  const response = await fetch('https://pokeapi.co/api/v2/pokemon?limit=' + limit)
  const data = await response.json()
  isLoading = false
  pokeList.value = await Promise.all(data.results.map(pokemon => fetchPokemonData(pokemon)))
}


async function fetchPokemonData(pokemon) {
  const response = await fetch(pokemon.url)
  const data = await response.json()
  return {
    id: data.id,
    name: data.name,
    typeOne: data.types[0].type.name,
    typeTwo: data.types[1]?.type?.name
  }
}

function capitalized(name) {
    if(name){
      const capitalizedFirst = name[0].toUpperCase();
      const rest = name.slice(1);
      return capitalizedFirst + rest;
    } else{
      return
    }
}

function pokeImage(pokeID){
  return 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/' + pokeID + '.png'
}

function nextPage(){
  isLoading = true
  limit = limit + 20
  if(limit > 1010){
    limit = 1010
    isLoading = false
  }
  fetchKantoPokemon()
}

onMounted(() => {
  fetchKantoPokemon()
  setTimeout(() => {
      isLoading = false;
    }, 1500);
})
</script>


<template>
  <LoadingScreen v-if="isLoading"/>
  <div></div>
  <div></div>
  <div></div>
  <div id="themeToggle">
    <ThemeToggle v-if="!isLoading" />
  </div>
  <div v-for="p in pokeList" :key="p.id">
    <div id="poke-container">
      <div id="top">
      <h3>{{ capitalized(p.name) }}</h3>
      </div>
      <div id="center">
        <img id="pokeImage" :src="pokeImage(p.id)">
      </div>
      <div id="bottom">
      <p>Pokedex Number: {{ p.id }}</p>
      <p>Types: {{ capitalized(p.typeOne) }} {{ capitalized(p.typeTwo) }}</p>
    </div>
    </div>
  </div>
  <div><Waypoint @change="nextPage()"><div><LoadingScreen id="smallLoader" v-if="isLoading"/></div></Waypoint></div>
</template>

<style scoped>

#smallLoader{
  line-height: 0;
  margin: 0;
}

h3{
  font-weight: bold;
}

#center{
  text-align: center;
}

#pokeImage{
  position: relative;
  margin: auto;
  margin-bottom: -1.5em;
}

#top{
    margin-top: 15%;
    text-align: center;
    color: white;
  }

  #bottom{
    color: black;
    padding: 12px;
  }

  #poke-container {
    background: linear-gradient(180deg, red 50%, white 50%);
    color: black;
    border-style: solid; 
    border-radius: 12px;
    max-width: 250px;
    max-height: 250px;
  }

  #themeToggle{
    padding-bottom: 24px;
    margin-right: 0;
    margin-left: auto;
  }

</style>
