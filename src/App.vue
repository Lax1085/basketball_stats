<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <seasonAverages :players="players"/>
  </div>
</template>

<script>
import seasonAverages from './components/seasonAverages.vue'

export default {
  name: 'App',
  components: {
    seasonAverages
  },
  data(){
      return {
        players: [],
        isLoading: true
      }
  },
  methods:{
    async getPlayers(){
      try{
        const response = await fetch("https://www.balldontlie.io/api/v1/players/237")
        const data = await response.json()
        //this.players={data};
        this.players = [...this.players, data];
        console.log(this.players);
        this.isLoading=false
      }catch(error){
        console.error(error);
      }
    }
  },
  mounted(){
    this.getPlayers();
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
</style>
