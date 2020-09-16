<template>
  <div class="pokedex">

    <h1>{{ msg }}</h1>

    <select name="poke-tri" id="poke-select">
      <option value="">--Order list by--</option>
      <option v-on:click="this.pokemon.sort((a, b) => a.name.localeCompare(b.name))" value="Name">Name</option>
      <option v-on:click="this.pokemon.sort((a, b) => (a.types==b.types))" value="Type">Type</option>
      <option v-on:click="this.pokemon.sort((a, b) => (a.id-b.id))" value="Id">Id</option>
    </select>

    <p>Rechercher un pokemon</p><input type="text" placeholder="par nom ou par type"><button>Rechercher</button>

    <p>Le poids total des 20 pokemons est {{totalWeight}}</p>

    <ul class="list">
      <li>
        <ul class="list-item" v-for="item in pokemon" :key="item">
          <li>
            <img alt="Image de Pokemon" v-bind:src="item.image"> 
          </li>
          <li class="item-center" id="item-name">{{item.name}}</li>
          <li class="item-center" id="item-types">{{item.types}}</li>
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
      totalWeight:Number,
    })
  },
  mounted(){
    this.getName20().then( //.then permet d'attendre la fin de la requête get pour ensuite exécuter la fct qui recupere les data 
        (data)=>
          {for(let id = 0 ; id<20 ; id++)
            {this.getName(data[id])}
    }
    )},
  methods: {
    /*
    search: function(){
      let type = []
      let saisie =document.getElementById("input").value;
      for (let i = 0; i < this.pokemon.length; i++) {
        if (this.pokemon[i].name == saisie) {
          return document.getElementById("item-name").innerHTML()
          }
        if (this.pokemon[i].types == saisie) {
          return document.getElementById("item-name").innerHTML(
          )
          }
      }
    },*/
    /*
    search:function(){
      let saisie = document.getElementById("input").value;
      if (this.pokemon.filter(a => a.name==saisie)) 
    },*/
    async getName20() {
      // fct qui sert à récupérer les 20 premiers url
      //Le résultat est recopié dans le tableau url
      //il sert ensuite à récupérer les informations pokemons
      try{
        const response = await axios.get("https://pokeapi.co/api/v2/pokemon?results=20")
        let url=[]
        for(let id=0 ; id<20 ; id++){
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
          let values = this.pokemon.map(poke => poke.weight)
          this.totalWeight = values.reduce((accumulator, currentValue) => { return accumulator + currentValue }, 0)
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
  margin: 0 10px;
}
a {
  color: #42b983;
}


.list{
  position: relative;
}
.list-item{
  margin:auto;
  display: flex;
  flex-direction: row;
  flex:7;
  height: 100px;
  width: 600px;
  border-style: solid;
  border-width: 1px;
  border-color:darkred;
}

.list-item img{
  flex:2;
  width: 50px;
}

.item-center{
  flex: 4;
  flex-direction: row;
}

.item-right{
  flex: 1;
}

#item-name{
  font-size:20px;
  font-weight: bold;
}
#item-types{
  font-size: 15px;

}
button{
  width: 100px;
  margin: 5px;
}
</style>
