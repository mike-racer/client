<template>
  <div class="container mt-1">

    <div class="row d-flex justify-content-center">
      <h1 class="display-2 col-lg-12 col-md-12 col-sm-12"><i class="fas fa-person-booth"></i><strong> {{room}}</strong></h1>
      <img src="http://i.imgur.com/CnPFg9W.jpg" class="rounded mb-5" style="max-height:400px">
      <div class="row">
      
      </div>
      <h3 class="display-4 col-lg-12 col-md-12 col-sm-12">Who Are You?</h3>
    </div>
    <p class="note"> {{notif}} </p>
    <div class="col-md-12">
      <input type="text" v-model="username" placeholder="Player name">
      <div class="row">
        
      </div>
      <button @click="login" class="btn btn-danger">ENTER RACE</button>
      <br><br>
      
    </div>
  </div>
</template>

<script>
import database from '../assets/config'

export default {
  name: 'login',
  data () {
    return {
      room: '',
      username: '',
      notif: ''
    }
  },
  methods: {
    login () {
      localStorage.setItem('username',this.username)
    
      let self = this
      if (self.username === '') {
        this.notif = 'Enter your name first'
      } else {
        database.ref('room/' + self.room + '/users').push({
          username: self.username,
          status: 'playing',
          index : 0
        })
          .then(response => {
            // console.log('whoami', response.key)
            localStorage.setItem('userid', response.key)
          })
        self.username = ''
        self.$router.push('/room')
      }
    },
    checkRoom () {
      let roomvalue = localStorage.getItem('room')
      this.room = roomvalue
    }
  },
  created () {
    this.checkRoom()
  }
}
</script>

<style scoped>
.welcome {
  margin-bottom: 30px;
}

.about {
  margin: 0 auto;
  width: 500px;
}

input, button {
  width: 100%;
  text-align: center;
}

.note {
  color: red;
}
</style>
