<template>
  <div class="home">
    <form>
      <input type="text" v-model="userID" placeholder="userID">
      <input type="text" v-model="email">
      <button @click.prevent="testWrite"> submit</button>
      {{ allPlayers }}
    </form>
  </div>
</template>

<script>
// @ is an alias to /src
import database from '@/assets/config.js';
import HelloWorld from '@/components/HelloWorld.vue'

const db = database

export default {
  data() {
    return {
      userID: "",
      email: "",
      allPlayers: []
    }
  },
  methods: {
    // testWrite: function () {
    //     db.ref('/db/room1/' + this.userID).push({
    //     email: this.email
    //     }, function(err) {
    //       if (err) {
    //         console.log(err);
    //       }
    //       else {
    //         console.log('data saved successfully');
    //       }
    //     })
    // },
    testRead: function () {
      db.ref('/room1').on('value', (snapshot) => {
        if (snapshot.val()) {
          console.log(snapshot.val());
          this.allPlayers = snapshot.val()
        }
      })
    }
  },
  mounted() {
    this.testRead()
  }
}
</script>
