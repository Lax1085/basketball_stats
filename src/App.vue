
<template>
  <div id="app">
    <div v-if="!isLoading">
      <seasonAverages :players="players"/>
      <lastTenGames :players="players"/>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import { BootstrapVue, IconsPlugin } from 'bootstrap-vue'
import seasonAverages from './components/seasonAverages.vue'
import lastTenGames from './components/lastTenGames.vue'
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'
// Install BootstrapVue
Vue.use(BootstrapVue)
// Optionally install the BootstrapVue icon components plugin
Vue.use(IconsPlugin)
export default {
  name: 'App',
  components: {
    seasonAverages,
    lastTenGames
  },
  data(){
      return {
        players: [],
        isLoading: true
      }
  },
  methods:{
    async getPlayerData(playerID){
      try{
        let url="https://www.balldontlie.io/api/v1/players/"+playerID
        console.log(url);
        const response = await fetch(url)
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
    /*for(const value of this.initPlayers){
      this.getPlayers(value);
    }*/
  },
  created(){
    if(this.players.length==0){
      this.getPlayerData(237)
      
    }
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
