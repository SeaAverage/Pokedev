<script setup>
import { onMounted } from 'vue';
import {ref} from 'vue';
import ThemeToggle from './ThemeToggle.vue';


const pokeList = ref([])


function fetchKantoPokemon(){
 fetch('https://pokeapi.co/api/v2/pokemon?limit=151')
  .then(response => response.json())
  .then(function(allpokemon){
  allpokemon.results.forEach(function(pokemon){
    fetchPokemonData(pokemon); 
  })
 })
}

function fetchPokemonData(pokemon){
let url = pokemon.url // <--- this is saving the pokemon url to a variable to us in a fetch.(Ex: https://pokeapi.co/api/v2/pokemon/1/)
  fetch(url)
  .then(response => response.json())
  .then(function(pokeData){
    pokeList.value.push({ id: pokeData.id, name: pokeData.name, typeOne: pokeData.types[0].type.name, typeTwo: pokeData.types[1]?.type?.name})
  })
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

onMounted(() => {
  fetchKantoPokemon()
})
</script>


<template>
  <div></div>
  <div></div>
  <div></div>
  <div id="themeToggle">
    <ThemeToggle />
  </div>
  <div id = pokeball, v-for="p in pokeList">
    <div id = poke-container>
      <div id= top>
      <h3>{{ capitalized(p.name) }}</h3>
      </div>
      <img id = pokeImage :src="'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/' + p.id + '.png'">
      <div id= bottom>
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

#pokeImage{
  margin-left: 25%;
  margin-right: 25%;
  margin-bottom: -25px;
}

#top{
    margin-top: 15%;
    text-align: center;
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
  }

  #themeToggle{
    padding-bottom: 24px;
    margin-right: 0;
    margin-left: auto;
  }

</style>
