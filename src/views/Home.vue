<template>
  <div class="home container">
    <!--<HelloWorld msg="Welcome to Your Vue.js App"/>-->
    <div class="row">
      <div class="col-8 border">
        <h3>MIKE RACER</h3>
        Insert Room Name :
        <input type="text" v-model="room"/>
        <button @click="setRoom">Join Room</button>
        <br>
        Room With Player :
        <div class="row">
          <list :roomList="roomList" v-on:sender= "joinRoom($event)"/>
        </div>
        <img class="footer" src="http://showgirlequestrian.com.au/wp-content/uploads/2016/02/Horses-Running.png">
      </div>
      <div class="col-2">
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import database from '@/assets/config.js'
import list from '@/components/listRoom.vue'
import buttons from '@/components/button.vue'

export default {
  name: 'home',

  data(){
    return{
      room:'',
      roomList:''
    }
  },
  components: {
    HelloWorld,
    list,
    buttons
  },
  methods: {
    setRoom(){
      let self = this
      if (self.room) {
        let racearena = database.ref('room/'+ self.room)
        racearena.once('value', function(snapshot){
          let result = snapshot.val()
          console.log('check room', result)
          if (!result) {
            database
              .ref('room/'+self.room)
              .set({
                name: self.room
              })
              .then(response => {
                console.log(response)
                localStorage.setItem('room', self.room)
                self.$router.push('login')
              })
          } else {
            let roomusers = result.users
            if (roomusers && Object.keys(roomusers).length >= 2){
              localStorage.removeItem('room')
              alert('Full Room. Please choose another!')
              window.location.reload()
            } else {
              localStorage.setItem('room', self.room)
              self.$router.push('login')
            }
          }
        })
      }
    },

    getAllRoom(){
      let self = this
      database.ref('room/').once('value', function(snapshoot){
        console.log('get all room', snapshoot.val())
        let result = snapshoot.val()
        self.roomList = Object.keys(result)
      })
    },

    joinRoom(room){
      this.room = room
     this.setRoom()
    }
  },

  created(){
    this.getAllRoom()
  }
}
</script>
<style scoped>
.iniclass{
  background: red;
  border-color: red
}
</style>

