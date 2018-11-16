<template>
    <div class="container mt-5">
        <div class="row">
            <div class="col-2">

            </div>

            <div class="col-8">
                <h1>Welcome to room {{ room }}</h1>
                <form class="form-inline my-5">
                    <div class="form-group mb-2">
                        <input type="text" class="form-control-plaintext" placeholder="input player name" v-model="username">
                    </div>
                    <button type="submit" class="btn btn-primary mb-2" @click.prevent="addPlayer">Add Player</button>
                </form>
                <br><br>
                {{ alert }}

                <p>Current Player: </p>
                <ul>
                    <li v-for="player in allPlayers" style="list-style:none"> {{ player.username }}</li>
                </ul>
            </div>

            <div class="col-2">

            </div>
        </div>
    </div>
</template>

<script>
// @ is an alias to /src
import database from '@/assets/config.js';

const db = database

export default {
  data() {
    return {
      username: "",
      room: "",
      allPlayers: [],
      alert: ""
    }
  },
    methods: {
        addPlayer: function () {
            let self = this
            if (self.username == '') {
                this.alert = 'Please enter your name'
            }
            else {
                this.alert = ''
                db.ref('room/' + self.room + '/users').push({
                    username: self.username
                })
                .then( response => {
                    localStorage.setItem('userID', response.key)
                    self.getPlayer()
                })
                self.username = ''
            }
        },
        checkRoom() {
            let roomValue = localStorage.getItem('room')
            this.room = roomValue
        },

        getPlayer: function() {
            let self = this
            let roomName = localStorage.getItem('room')
            console.log('this is room name', roomName);
            
            db.ref('room/' + roomName + '/users').once('value', function (snapshot) {
                let result = snapshot.val()
                self.allPlayers = Object.values(result)
                console.log(self.allPlayers);
            })
        }

    },


    created() {
        this.checkRoom()
        this.getPlayer()
    }

}
</script>
