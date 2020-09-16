<template>
  <div class="pokedex">

    <h1>{{ msg }}</h1>
    
    <select name="poke-tri" id="poke-select">
      <option value="">--Order list by--</option>
      <option value="Name">Name</option>
      <option value="Type">Type</option>
      <option value="Id">Id</option>
    </select>
    <ul class="list">
      <li>
        <ul class="list-item" v-for="item in pokemon" :key="item">
          <li>
            <img alt="Image de Pokemon" src="${item.image}">
          </li>
          <li>{{item.name}}</li>
          <li>{{item.types}}</li>
          <li>{{item.id}}</li>

        </ul>
      </li>
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
  computed:{
    getImg:function(name){
      let asset = "../assets/"+name
      return asset
    },/*
    orderbyName: function() {
    let tmp = this.pokemon.name
    tmp.sort()
    return tmp.sort()
  },
    orderbyId: function() {
    let tmp = this.pokemon.id
    return tmp.sort()
    }*/
    },
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
            types:types,
            image:"../assets/"+response.data.name+".png"
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
.list{
  border-style: solid;
  position: relative;
  justify-content: center;
}
.list-item{
  border-style: solid;
  position: relative;
  border-width: 1px;
  border-color:darkred;
  padding: 10px;
  margin: 5px;
}
</style>
