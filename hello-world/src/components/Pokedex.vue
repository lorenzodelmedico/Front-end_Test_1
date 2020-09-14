<template>
  <div class="pokedex">
    <h1>{{ msg }}</h1>
    <p id="cache"> {{info}}</p>
    <ul :key="item" v-for="(item) in info.name">
      <li>{{item}}</li>
    </ul>


  </div>
</template>

<script>

const axios = require("axios");

export default {
  name: 'Pokedex',
  props: {
    msg: String
  }, 
  data (){
    return ({
      info:{
        name:{},
        url:{},
        pokemon:{
          id:"",
          types:{},
          height:"",
          weight:"",
          ability:{},
        }
      },
      error: {},
    })
  },
  created(){
    //methode qui sert à appeler l'appel API avant que le premier render soit effectué
    this.getName20();
  },
  computed:{
    filterbyName: function() {
    return this.info.name.sort(function(a, b) {
      var nameA = a.name.toUpperCase(); // ignore upper and lowercase
      var nameB = b.name.toUpperCase(); // ignore upper and lowercase
      if (nameA < nameB) {
        return -1;
      }
      if (nameA > nameB) {
        return 1;
      }
      // names must be equal
      return 0;
    })
    }
  },
  mounted(){
    this.getName("https://pokeapi.co/api/v2/pokemon/1/")
  },
  methods: {
    async getName20() {
      // methode qui sert à récupérer les 20 premiers noms, et url de l'api Pokemon
      //Le résultat est recopié dans l'objet "info"
      try{
        const response = await axios.get("https://pokeapi.co/api/v2/pokemon?results=20")
        var id = 0
        for(id=0 ; id<20 ; id++){
          this.info.name[id+1] = response.data.results[id].name 
          this.info.url[id+1] = response.data.results[id].url
          }
      }
      catch (e){
          this.error.push(e)
      }
    },
    async getName (url) {
      try{
          const response = await axios.get(url)
          this.info.pokemon.id = response.data.id
          this.info.pokemon.height = response.data.height
          this.info.pokemon.weight = response.data.weight
          var id = 0
          for (id=0 ; id<response.data.abilities.length ; id++){
            this.info.pokemon.ability[id+1] = response.data.abilities[id].ability.name
          }
          for (id=0 ; id<response.data.types.length ; id++){
            this.info.pokemon.types[id+1] = response.data.types[id].type.name
          }    
      }
      catch (e){
          this.error.push(e)
      }
    },  
  }
}

</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
#cache{
  display: none;
}
</style>
