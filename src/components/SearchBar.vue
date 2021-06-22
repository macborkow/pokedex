<template>
<div id='searchbar'>
<input type='text' v-model="regex"/>
<button @click="handleClick(item.name)" v-for="item, index in filteredList" :key='index'>
{{item.name.charAt(0).toUpperCase() + item.name.slice(1)}}
</button>
</div>
</template>

<script>
export default {
  name: 'SearchBar',
  props: {
    list: Array,
  },
  data: function() {
    return {
      regex: "",
      filteredList: []
    }
  },
  watch: {
    regex: function(val) {
      let count = 0;
      this.filteredList = this.list.filter( item => {
        if(val!= "" && item.name.match(new RegExp(`^${val}`,'i')) && count < 5) {
        count++
        return true
      }})
    }
  },
  methods: {
    handleClick(val) {
      this.regex=""
      this.$emit('clicked', val)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
input, select {
  background-color: #fff9ff;
  font-size: 3vh;
}
#searchbar {
  margin: 10px auto;
  display: flex;
  flex-direction: column;
  width: 200px;
}
#searchbar>* {
  background-color: #fff9ff;
  font-size: 3vh;
}
</style>
