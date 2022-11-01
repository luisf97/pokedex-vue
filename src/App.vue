<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">

      <h3 class="title is-3">Pokedex</h3>
      <input type="text" class="input is-rounded" placeholder="Buscar um pokemon pelo nome" v-model="busca" >

      <button class="button is-info is-outlined is-medium is-fullwidth" id="buscarBtn">Buscar</button>

      <div v-for="(poke, index) in resultadoBusca" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :index="index + 1"/>
      </div>
    </div>

  </div>
</template>

<script>

import axios from 'axios'
import Pokemon from '@/components/Pokemon'

export default {
  name: 'App',

  data(){
    return {
      pokemons: [],
      busca: ''
    }
  },

  created() {
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151$offset=0").then(({data: {results}}) => {

      this.pokemons = results;

    }).catch(err => console.error(err))
  },
    components: {
    Pokemon
  },
  computed: {
    resultadoBusca: function() {
      if(this.busca == '' || this.busca == ' ') {
        return this.pokemons
      } else {
        return this.pokemons.filter(pokemon => pokemon.name == this.busca)
      }
    }    
  }
}
</script>

<style scoped>

  #buscarBtn {

    margin-top: 1em;
  }

</style>

