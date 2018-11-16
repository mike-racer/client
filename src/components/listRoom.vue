<template>
    <div>
        <div class="row">
            <div v-for="(room, index) in roomList" :key="index" class="col-md-3">
                <div class="card" style="width: 18rem;">
                    <img class="card-img-top" src="../assets/mikey.png" alt="Card image cap">
                    <div class="card-body">
                    
                        <h5 class="card-title">{{ room }}</h5>
                        <button class="buttonStyle" @click="$emit('sender', room)">join</button>
                    </div>
                </div>         
            </div>
        </div>      
    </div>
</template>
<script>
// import buttons from '@/components/button.vue'
export default {
    name: 'listRoom',
    props: ['roomList'],
    components:{
    }

}
</script>
<style scoped>
.card-body{
    left: 100px;
}

.mike{
    border: 200px;
    border-color: red;
}
.card{
    border-radius: 10px;
    margin: 10px;
}
.buttonStyle{
    background: transparent;
    color: blue;
    border-color: blue;
    border-radius: 5px;
}
.buttonStyle:hover{
    background: transparent;
    color: red;
    border-color: red;
    border-radius: 5px;
    font-size: 20px;
}
.buttonStyle:active{
    background: transparent;
    color: black;
    border-color: black;
    border-radius: 5px;
    font-size: 20px;
}


</style>
