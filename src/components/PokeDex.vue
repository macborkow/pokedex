<template>
<div id="gameboy">
<h1>PokéSearch</h1>
<!--<select v-model="choice">
  <option :key="item.url" v-for="item in pokes" :value="item"> {{ item.name }} </option>
</select>-->
<SearchBar @clicked="handleClicked" :list="pokes"/>
<div v-if="pic">
  <div id="pokeBorder">
    <p id="pokeName"> {{choice.name.charAt(0).toUpperCase()+choice.name.slice(1)}} </p>
    <img :src="pic" alt="pokemon">
    <p> Height: {{height}} </p>
    <p> Weight: {{weight}} </p>
    <p> Type{{type.length>1?'s':''}}: <span v-for="it in type" :key="it">{{it}} </span> </p>
  </div>
  <div id="attackSearch">
    <p style="margin-right: 10px;">Attacks:</p>
    <input placeholder="filter attacks" type="text" v-model="pattern">
  </div>
  <div id="flex"><div v-for="move in searchedMoves" :key="move">{{move}}</div></div>
</div>
</div>
</template>

<script>
import axios from 'axios'
import SearchBar from './SearchBar.vue'

export default {
  name: 'PokeDex',
  components: {SearchBar},
  mounted: async function(){
    await axios.get('https://pokeapi.co/api/v2/pokemon?limit=151&offset=0')
    .then(res => { console.log(res); res.data.results.forEach(item => { this.pokes.push({name: item.name, url: item.url}) }) } )
  },
  data: function() {
    return {
      pokes: [],
      choice: "",
      pic: "",
      height: "",
      weight: "",
      type: [],
      moves: [],
      searchedMoves: [],
      pattern: ""
    }
  },
  methods: {
    handleClicked(val) {
      console.log(val)
      this.choice = this.pokes.find( item => item.name == val)
    }
  },
  watch: {
    choice: async function(val) {
      await axios.get(val.url)
      .then(res => {
        console.log(res)
        this.type = []
        this.moves = []
        this.pic = res.data.sprites.versions["generation-v"]["black-white"].animated.front_default
        this.height = res.data.height
        this.weight = res.data.weight
        res.data.types.forEach(item => {
          this.type.push(item.type.name)
        })
        res.data.moves.forEach(item => {
          this.moves.push(item.move.name)
        })
        this.searchedMoves = this.moves;
        this.pattern = ""
      })
    },
    pattern: function(val) {
      this.searchedMoves = this.moves.filter( item => {
        if(item.match(val))
          return item
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
* {
  font-family: 'VT323', monospace;
  letter-spacing: 3px;
}
</style>
<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Bangers&family=VT323&display=swap');
#pokeBorder {
  margin: 0 auto;
  padding: 0px 20px;
  width: 30%;
  border: 5px solid #84739c;
  border-left: 5px solid black;
  border-top: 5px solid black;
  background-color: #fff9ff;
}
input, select {
  background-color: #ffefff;
}
select {
  border: none;
}
p {
  color: #181010
}
h1 {
  font-family: 'Bangers', cursive;
  margin: 0;
  letter-spacing: 6px;
  margin-top: 30px;
  font-size: 15vh;
  color: #fccd04;
  -webkit-text-stroke: #365aa5 1.5px;
}
#flex {
  display: flex;
  flex-wrap: wrap;
  margin: 0 auto;
  justify-content: center;
}
#flex>* {
  padding: 7px;
  margin: 5px;
  color: white;
  background-color: #84739c ;
}
#gameboy {
  letter-spacing: 2px;
  background-color: #ffefff ;
  border: 5px solid #84739c;
  border-left: 5px solid black;
  border-top: 5px solid black;
  padding-bottom: 10px;
  width: 60%;
  margin: 0 auto;
}
#attackSearch {
  justify-content: center;
  height: 50px;
  align-items: center;
  display: flex;
}
</style>
