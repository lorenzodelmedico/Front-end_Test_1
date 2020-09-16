<template>
  <div class="pokedex">
      <p>{{height}}</p>
      <p>{{weight}}</p>
      <ul v-for="item in abilities" :key="item">
          <li>{{item}}</li>
      </ul>
    
  </div>
</template>

<script>

//composant PokeInfo, affiche les informations passé en prop par Pokedex lors d'un évènement click


export default {
  name: 'PokeInfo',
  props: {
    height:String,
    weight:String,
    abilities:[]
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
</style>
