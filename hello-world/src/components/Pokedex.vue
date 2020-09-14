<template>
  <div class="pokedex">
    <h1>{{ msg }}</h1>
    <ul>
      <li>{{url[0]}}</li>
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
      pokemon:[], //utilisation d'un tableau d'objets 
      error: [],
    })
  },
  /*
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
  },*/
  mounted(){
    this.getName20().then( //.then permet d'attendre la fin de la requête get pour ensuite exécuter la fct qui recupere les data 
        (data)=>
          {for(let id = 0 ; id<20 ; id++)
            {this.getName(data[id])}
    }
    )},
  methods: {
    async getName20() {
      // methode qui sert à récupérer les 20 premiers url
      //Le résultat est recopié dans le tableau url
      //il sert ensuite à récupérer les informations pokemons
      try{
        const response = await axios.get("https://pokeapi.co/api/v2/pokemon?results=20")
        var id = 0
        let url=[]
        for(id=0 ; id<20 ; id++){
          url.push(response.data.results[id].url)
          }
        return url
      }
      catch (e){
          this.error.push(e)
      }
    },
    async getName (url) {
      try{
          const response = await axios.get(url)
          let ability = []
          let types = []
          for (let id=0 ; id<response.data.abilities.length ; id++){
            ability.push(response.data.abilities[id].ability.name)
          }
          for (let id=0 ; id<response.data.types.length ; id++){
            types.push(response.data.types[id].type.name)
          }  
          this.pokemon.push({ //construction d'un objet Json, push dans le tableau 
            id:response.data.id,
            name:response.data.name,
            height:response.data.height,
            weight:response.data.weight,
            ability:ability,
            types:types
          })
            
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
