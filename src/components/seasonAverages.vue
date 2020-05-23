<template>
<div id="seasonAverages" v-if="players.length > 0" class="test">
<h4>'18-'19 Season Averages</h4>
<div v-for="player in players" :key="player.id">
  <h5>{{player.first_name}} {{player.last_name}}</h5>
  <h6>Position: {{player.position}} | Team:
    <!--<img v-bind:src="'./images/' + player.team.abbreviation + '.gif' " />-->



     {{player.team.full_name}} </h6>
    <p v-if="isLoading">
      Loading
    </p>
    <table class="table" v-else-if="playerAverages[player.id]" :data="playerAverages">
      <thead>
        <tr>
        <th scope="col">GP</th>
        <th scope="col">MIN</th>
        <th scope="col">FGM</th>
        <th scope="col">FGA</th>
        <th scope="col">FGPCT</th>
        <th scope="col">FG3M</th>
        <th scope="col">FG3A</th>
        <th scope="col">FG3PCT</th>
        <th scope="col">REB</th>
        <th scope="col">AST</th>
        <th scope="col">STL</th>
        <th scope="col">BLK</th>
        <th scope="col">PTS</th>
        </tr>
      </thead>
      <tbody >
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
      </tbody>
    </table>
    <p v-else>
       No stats found. {{isLoading}}
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
        this.isLoading=false
        //console.debug(this.playerAverages);
      }catch(error){
          console.error(error);
      }
    }
  },
  mounted(){
    this.getSeasonData();
  }
}
</script>
