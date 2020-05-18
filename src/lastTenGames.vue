<template>
<div id="seasonAverages" v-if="players.length > 0" class="test">
<h4>Season Averages</h4>
<div v-for="player in players" :key="player.id">
  <h5>{{player.first_name}} {{player.last_name}} </h5>
  <h6>Position: {{player.position}} | Team: {{player.team.abbreviation}} </h6>
    <p v-if="isLoading">
      isLoading
    </p>
    <table v-if="playerAverages[player.id] && !isLoading" :data="playerAverages">
      <tr>
        <th>GP</th>
        <th>MIN</th>
        <th>FGM</th>
        <th>FGA</th>
        <th>FGPCT</th>
        <th>FG3M</th>
        <th>FG3A</th>
        <th>FG3PCT</th>
        <th>REB</th>
        <th>AST</th>
        <th>STL</th>
        <th>BLK</th>
        <th>PTS</th>
      </tr>
      <tr>
        <td>{{playerAverages[player.id].games_played}}</td>
        <td>{{playerAverages[player.id].min}}</td>
        <td>{{playerAverages[player.id].fgm}}</td>
        <td>{{playerAverages[player.id].fga}}</td>
        <td>{{playerAverages[player.id].fg_pct}}</td>
        <td>{{playerAverages[player.id].fg3m}}</td>
        <td>{{playerAverages[player.id].fg3a}}</td>
        <td>{{playerAverages[player.id].fg3_pct}}</td>
        <td>{{playerAverages[player.id].reb}}</td>
        <td>{{playerAverages[player.id].ast}}</td>
        <td>{{playerAverages[player.id].stl}}</td>
        <td>{{playerAverages[player.id].blk}}</td>
        <td>{{playerAverages[player.id].pts}}</td>
      </tr>
    </table>
    <p v-else>
       No stats
    </p>
</div>
</div>
<div v-else class="empty-table">
  <p >
     No players
  </p>
</div>
</template>

<script>
export default{
  name: 'seasonAverages',
  data(){
      return {
        playerAverages: {},
        isLoading: true
      }
  },
  props: {
    players: Array,
  },
  methods:{
    async getSeasonData(){
      let playerArray=[]
      for(const value of this.players) {
        playerArray=[...playerArray,value.id]
      }
      //console.debug(playerArray)
      try{
        let url="https://www.balldontlie.io/api/v1/season_averages?season=2018"
        for(const value of playerArray){
          url+="&player_ids[]="+value;
        }
        console.log(url);
        const response =  await fetch(url)
        const data = await response.json()
        for(const value of data.data) {
          this.playerAverages[value.player_id]= value;
        }
        console.debug(this.playerAverages);
        this.isLoading=false
      }catch(error){
          console.error(error);
      }
    }
  },
  updated(){
    this.getSeasonData();
  }
}
</script>
