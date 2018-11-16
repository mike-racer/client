<template>
  <div class="container">
    <div class="row mt-5 d-flex justify-content-center">
      <h1 class="display-1 col-sm-12 col-md-12 col-lg-12">MIKE RACER</h1>
      <img class="col-sm-12 col-md-12 col-lg-12" src="http://showgirlequestrian.com.au/wp-content/uploads/2016/02/Horses-Running.png">
      <div class="col-sm-12 col-md-12 col-lg-12 mt-3">
        <div class="row d-flex justify-content-center">
          <h5 class="h5 col-sm-12 col-md-12 col-lg-12"><strong>ROOM LIST : </strong></h5>
          <div @click="selectRoom(single)" class="col-sm-4 col-md-3 col-lg-2 border btn dusty-grass-gradient text-dark" v-for="(single, index) in roomlist" :key="index" style="cursor:pointer">
            <h5 class="h5">{{single}}</h5>
          </div>
        </div>
        <h4 class="mt-2">Selected Room : {{room}}</h4>
        <input type="text" class="form-control mt-3" placeholder="CREATE ROOM" v-model="room"><br>
        <button class="btn btn-success" @click="setRoom">Enter Room</button>
      </div>
    </div>
    <br>
  </div>
</template>

<script>
import database from '../assets/config'

export default {
  name: 'home',
  data () {
    return {
      room: '',
      roomlist: []
    }
  },
  methods: {
    setRoom () {
      let self = this
      if (self.room) {
        let hangroom = database.ref('room/' + self.room)
        hangroom.once('value', function (snapshot) {
          let result = snapshot.val()
          console.log('check room', result)
          if (!result) {
            database
              .ref('room/' + self.room)
              .set({
                name: self.room
              })
              .then(response => {
                console.log(response)
                localStorage.setItem('room', self.room)
                self.$router.push('loginv2')
              })
          } else {
            let roomusers = result.users
            if (roomusers && Object.keys(roomusers).length >= 2) {
              localStorage.removeItem('room')
              alert('Full Room. Please choose another!')
            } else {
              localStorage.setItem('room', self.room)
              self.$router.push('loginv2')
            }
          }
        })
      }
    },
    getAllRoom () {
      let self = this
      database.ref('room/').on('value', function (snapshot) {
        console.log('get all room', snapshot.val())
        let result = snapshot.val()
        self.roomlist = Object.keys(result)
      })
    },
    selectRoom(roomName){
      this.room = roomName
    }
  },
  created () {
    this.getAllRoom()
  }
}
</script>

<style scoped>
.home {
  color: rgb(134, 5, 5);
  background-color: black;
}

#banner {
  font-family: 'Butcherman', cursive;
  font-size: 120px;
}

#scarecrow {
  width: 25rem;
  margin-right: 90px !important;
  border: 10px solid darkred !important
}

#subtitle {
  font-family: 'Oswald', sans-serif;
  font-size: 21px;
  color: white;
}

#alert-subtitle {
  background-color: rgb(134, 5, 5);
  border-radius: 0px !important;
  width: 680px !important;
}

input, button {
  width: 100%;
  text-align: center;
}

#oneroom:hover {
  background-color: #003366;
}

#divroom {
  background-color: rgb(134, 5, 5);
  color: white;
}

#btn-room {
  background-color: black;
  border: none !important;
  color: white;
}

#btn-room:hover {
  background-color: #ffa500;
  border: none !important;
  color: white;
}

ul, li {
  list-style: none;
}
</style>
