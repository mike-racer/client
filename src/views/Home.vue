<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <h3>MIKE RACER</h3>

    Room With Player :
    <ul>
      <li v-for="room in listRoom" :key="room.id"></li>
    </ul>
    Insert Room Name :
    <input type="text" v-model="list"/>
    <button @click="setRoom">Play</button>
     </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import database from '@/assets/config.js'

export default {
  name: 'home',

  data(){
    return{
      room:'',
      roomList:''
    }
  },
  components: {
    HelloWorld
  },
  methods: {
    setRoom(){
      let self = this
      if (self.room) {
        let racearena = database.ref('room/'+ self.room)
        racearena.once('value', function(snapshoot){
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
      database.ref('room/').once('value', function(snapshoot){})
    }
  }
}
</script>
