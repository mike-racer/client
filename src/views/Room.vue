<template>
  <div class="home">
    <div class="row">
    <div class="col-sm-3 col-md-2 col-lg-1 border-top border-bottom" style="width:60px">
        <h4 class="mt-4">START</h4>
    </div>
    <track-box :data="arena" v-for="(arena,index) in realtimearenas" :key="index"></track-box>
    <div class="col-sm-3 col-md-2 col-lg-1 border-top border-bottom" style="width:60px">
        <h4 class="mt-4">FINISH</h4>
    </div>
    </div>
    <!-- <h3>{{track}}</h3> -->
    <div v-if="startgame">
        <h1 v-if="track[index] === ' '"><strong>TAP SPACE </strong> </h1>
        <h1 v-if="track[index] !== ' '"><strong>TYPE : {{track[index]}}</strong></h1>
        <input v-if="gameOn" type="text" v-model="input">
    </div>
    <div v-if="!startgame && gameOn" class="row d-flex justify-content-center">
        <div class="col">
            <h3>WAITING FOR OTHER PLAYER....</h3>
        </div>
    </div>
    <div v-if="loser" class="row d-flex justify-content-center">
        <div class="col">
            <h3>YOU LOSE</h3>
        </div>
    </div>
    <div v-if="winner" class="row d-flex justify-content-center">
        <div class="col">
            <h3>YOU WIN</h3>
        </div>
    </div>
    
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import database from '../assets/config'
import TrackBox from '@/components/Track.vue'

export default {
  name: 'home',
  components: {
    HelloWorld,
    TrackBox,
  },
  data (){
    return {
      track : [' ','@',' ',' ',' ','&2!',' ',' ',' ',' ','(1@',' ',' ','!#!',' ',' ',' ',' ',' ',' ','*1#',' ','z',' ',' ',' ',' ','*',' ','!!!'],
      arenas : ['https://media.giphy.com/media/Cp9Siser6g0x2/giphy.gif',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' '],
      realtimearenas : [' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' '],
      index : 0,
      input : '',
      username : localStorage.getItem('username'),
      room : localStorage.getItem('room'),
      userid : localStorage.getItem('userid'),
      realtimeindex : 0,
      realtimedata : '',

      startgame : false,
      gameOn : true,
      loser : false,
      winner : false
    }
  },
  watch : {
    input : function(val){
      if(this.input === this.track[this.index]){
        this.playSound('http://soundbible.com/mp3/Horse Galloping-SoundBible.com-1451398148.mp3')
        this.input = ''
        this.track[this.index] = 'KUDA'
        this.arenas[this.index-1] = ''
        this.arenas[this.index] = 'https://media.giphy.com/media/Cp9Siser6g0x2/giphy.gif'
        this.track[this.index - 1] = ' '
        this.index = this.index + 1

        this.updateIndex()
      }else if(this.input !== this.track[this.index]){
        if(this.input === ' '){
          this.playSound('http://soundbible.com/mp3/Horse%20Neigh-SoundBible.com-1126369713.mp3')
          this.input = ''
        }
      }
    },
    index : function(val){
      if(this.index === this.track.length){
        this.arenas[this.index-1] = ''
        this.arenas[this.index] = 'https://www.wackyraces.net/wp-content/uploads/2016/09/Big-Win.png'
      }
    }
  },
  methods : {
    playSound (sound) {
      if(sound) {
        var audio = new Audio(sound);
        audio.play();
      }
    },
    updateIndex(){
        let ref = database.ref('room/' + this.room + '/users/' + localStorage.getItem('userid'))
        ref.update({
            index : this.index
        })
    },
    setWinner(name){
        if(name === localStorage.getItem('username')){
            let ref = database.ref('room/' + this.room + '/users/' + localStorage.getItem('userid'))
                ref.update({
                status : 'winner'
            })
            this.winner = true
        }else{
            let ref = database.ref('room/' + this.room + '/users/' + localStorage.getItem('userid'))
                ref.update({
                status : 'loser'
            })
            this.loser = true
        }
    }
  },
  mounted (){
    let self = this
    this.userid = localStorage.getItem('userid')

    let userlist = database.ref('room/' + this.room + '/users')
    userlist.on('value', function (snapshot) {
        let getStat = snapshot.val()
        let player = Object.values(getStat)
        let playerId = Object.keys(getStat)

        if(player.length > 1){
            self.startgame = true

            let p1 = player[0].index
            let p2 = player[1].index
            let p1n = player[0].username
            let p2n = player[1].username
            let p2stat = player[0].status
            let p1stat = player[1].status

            if(p1stat === 'winner'){
                self.gameOn = false
                self.startgame = false
            }else if(p2stat === 'winner'){
                self.gameOn = false
                self.startgame = false
            }
    
            self.realtimearenas = [' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ',' ']
            
            self.realtimearenas[p1] = `https://media.giphy.com/media/Cp9Siser6g0x2/giphy.gif`
            self.realtimearenas[p1] = {
                image : `https://media.giphy.com/media/Cp9Siser6g0x2/giphy.gif`,
                username : p1n
            }

            self.realtimearenas[p2] = `https://media.giphy.com/media/Cp9Siser6g0x2/giphy.gif`
            self.realtimearenas[p2] = {
                image : `https://media.giphy.com/media/Cp9Siser6g0x2/giphy.gif`,
                username : p2n
            }

            if(p1 === 30){
                self.setWinner(p1n)
            }else if(p2 === 30){
                self.setWinner(p2n)
            }

        }
        
    })
  },
  created(){
      this.checkUser()
  }
}
</script>

