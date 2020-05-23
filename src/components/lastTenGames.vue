<template>
<div id="lastTenGames" v-if="players.length > 0 && !isLoading" class="test">
<h4>Last Ten Games
  <select v-model="playerViewed" :val="0">
    <option v-for="(player, key) in players" :key="key" >
      {{player.first_name}} {{player.last_name}}
    </option>
  </select>
</h4>
<div v-for="player in players" :key="player.id" >
    <table class="table table-striped">
      <thead>
        <tr>
          <th scope="col">Opp</th>
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
      <tbody>
        <tr v-for="game in lastTenGames" :key="game.id">
          <td>{{ convertDate(game.date)   }}</td>
          <td>{{ game.stats ? getPlayersStats(game).min : 0}}</td>
          <td>{{ game.stats ? getPlayersStats(game).fgm : 0}}</td>
          <td>{{ game.stats ? getPlayersStats(game).fga : 0}}</td>
          <td>{{ game.stats ? getPlayersStats(game).fg_pct : 0}}</td>

          <td>{{ game.stats ? getPlayersStats(game).fg3m : 0}}</td>

          <td>{{ game.stats ? getPlayersStats(game).fg3a : 0}}</td>
          <td>{{ game.stats ? getPlayersStats(game).fg3_pct : 0}}</td>
          <td>{{ game.stats ? getPlayersStats(game).dreb : 0}}</td>
          <td>{{ game.stats ? getPlayersStats(game).ast : 0}}</td>
          <td>{{ game.stats ? getPlayersStats(game).stl : 0}}</td>
          <td>{{ game.stats ? getPlayersStats(game).blk : 0}}</td>
          <td>{{ game.stats ? getPlayersStats(game).pts : 0}}</td>
        </tr>
      </tbody>
    </table>
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
  name: 'lastTenGames',
  data(){
      return {
        lastTenGames: {},
        isLoading: true,
        playerViewed:0
      }
  },
  props: {
    players: Array,
  },
  computed:{

  },
  methods:{
    convertDate(dateString){
      let date = new Date(dateString)
      return         ((date.getMonth() > 8) ? (date.getMonth() + 1) : ('0' + (date.getMonth() + 1)))
              + '/' + ((date.getDate() > 9) ? date.getDate() : ('0' + date.getDate()))
              + '/' + date.getFullYear()
    },
    getrivalTeam(){
    },
    getPlayersStats(game){
      if(game.stats === 'undefined')
        return 0
      else {
        return game.stats
      }
    },
    async getLastTenGames(){
      try{
        var date = new Date();
        date.setYear("2019")

        let url="https://www.balldontlie.io/api/v1/games?per_page=100&seasons[]=2018&team_ids[]="
        url+=this.players[0].team.id;
        url+="&end_date="+date.toISOString().split('T')[0];
        console.log(url);
        const response = await fetch(url)
        const data = await response.json()
        this.lastTenGames=data.data.sort((a, b) => (a.date < b.date) ? 1 : -1)
        this.lastTenGames=this.lastTenGames.slice(0,9)

        for(var i=0;i<this.lastTenGames.length;i++){
          let url="https://www.balldontlie.io/api/v1/stats?game_ids[]="
          +this.lastTenGames[i].id+"&player_ids[]=237"
          console.log(url);
          let response = await fetch(url)
          let data =  await response.json()
          this.lastTenGames[i].stats=data.data[0];
        }
        //lastTenGames=data.


        this.isLoading=false


      }catch(error){
          console.error(error);
      }
    },
  },
  beforeCreate(){
    this.playerViewed=0;
  },
  mounted(){
    this.getLastTenGames();
  }
}
</script>
