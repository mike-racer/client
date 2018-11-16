<template>
    <div class="col-sm-3 col-md-2 col-lg-1 border-top border-bottom" style="min-height:100px">
        <img v-if="data" :src="data.image" style="width:50px">
        <p v-if="data"><strong>{{data.username}}</strong></p>
    </div>
</template>
<script>
export default {
    props : ['data'],
    mounted () {
        console.log('Track Mounted')
        console.log('data')
    },
    watch : {
        data : function(val){
            console.log(val)
        }
    }
}
</script>

