<template>
  <div id="app">
    <PokeDex />
  </div>
</template>

<script>
import PokeDex from './components/PokeDex.vue'

export default {
  name: 'App',
  components: {
    PokeDex
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
}
</style>
