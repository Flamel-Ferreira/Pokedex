<template>
  <div id="app">
    <header>
      <h1> Pokédex - PokéAPI</h1>
      <input type="text" placeholder="Nome do pokémon" v-model="busca">
      <button @click="buscar">Buscar</button>
    </header>
    <div class="visualizacao">
      <div v-for="(pokemon) in listaFiltradaPokemons" :key="pokemon.name">
        <cpokemon :name="pokemon.name" :url="pokemon.url"/>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import cpokemon from './components/cPokemon';
export default {
  name: 'App',
  components: {
    cpokemon
  },
  data(){
    return {
      listaPokemons: [],
      listaFiltradaPokemons:[],
      busca: ''
    }
  },
  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then( res => {
      // Armazenando a resposta da requisição numa array em data()
      this.listaPokemons = res.data.results;
      this.listaFiltradaPokemons = this.listaPokemons;
    }).catch(error => {
      alert(error);
    });
  },
  methods:{
    buscar: function(){
      this.listaFiltradaPokemons = this.listaPokemons;
      if(this.busca == '' || this.busca == ' '){
        this.listaFiltradaPokemons = this.listaPokemons;
      }else{
        this.listaFiltradaPokemons = this.listaPokemons.filter( pokemon => pokemon.name.includes(this.busca));
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  color: #2c3e50;
  margin-top:30px;
}
header{
  font-size: 40px;
  padding:2%;
  display:flex;
  gap: 0;
}
header input{
  padding: 5px 12px;
  border-radius: 10px;
  border: 1px solid #ccc;
  width: 100%;
}
header button{
  width: 100%;
  border-radius:30px;
  border:none;
  background-color:rgb(17, 165, 17);
  color:white;
  font-size:15px;
  padding:5px;
}

header button:hover{
  background-color:rgb(6, 135, 6);
  cursor:pointer;
}
.visualizacao{
  display:flex;
  flex-wrap:wrap;
  justify-content: space-evenly;
  gap: 20px;
  max-width:1100px;
  padding: 2%;
}
</style>
