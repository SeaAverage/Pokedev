<script setup>
import { onMounted } from 'vue';
import {ref} from 'vue';
import ThemeToggle from './ThemeToggle.vue';
import LoadingScreen from './LoadingScreen.vue';


const pokeList = ref([])
var isLoading = true


// function fetchKantoPokemon(){
//  fetch('https://pokeapi.co/api/v2/pokemon?limit=151')
//   .then(response => response.json())
//   .then(function(allpokemon){
//   allpokemon.results.forEach(function(pokemon){
//     fetchPokemonData(pokemon); 
//   })
//  })
// }

async function fetchKantoPokemon() {
  const response = await fetch('https://pokeapi.co/api/v2/pokemon?limit=151')
  const data = await response.json()
  isLoading = false
  pokeList.value = await Promise.all(data.results.map(pokemon => fetchPokemonData(pokemon)))
}

// function fetchPokemonData(pokemon){
// let url = pokemon.url // <--- this is saving the pokemon url to a variable to us in a fetch.(Ex: https://pokeapi.co/api/v2/pokemon/1/)
//   fetch(url)
//   .then(response => response.json())
//   .then(function(pokeData){
//     pokeList.value.push({ id: pokeData.id, name: pokeData.name, typeOne: pokeData.types[0].type.name, typeTwo: pokeData.types[1]?.type?.name})
//   })
// }

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
</template>

<style scoped>

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
