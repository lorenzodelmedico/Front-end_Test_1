<template>
  <div id="app">
    <img alt="Pokemon Professeur Chen" src="./assets/chen.png">
    <Pokedex msg="Bienvenue dans ton Pokédex"/>
  </div>
</template>

<script>
import Pokedex from './components/Pokedex.vue'

export default {
  name: 'App',
  components: {
    Pokedex
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

img{
  width: 7%;
}
</style>
