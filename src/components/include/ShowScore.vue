<template>
    <div class="card col-md-6" style="padding:30px">
        <div class="iconcontainer successicon" v-if="!this.showFail">
            <font-awesome-icon icon="check" :style="'font-size:60px'" />
        </div>

       <div class="iconcontainer failicon"  v-if="this.showFail">
        <font-awesome-icon icon="times" :style="'font-size:60px'"/>
       </div>
        <div class="score-container" style="padding-top:30px">
            <h1 class="text-center">Your Score</h1>
            <h2 class="text-center">{{this.$props.score}} %</h2>
        </div>
        <div class="actioncontainer text-center" style="padding-top:10px">
            <button class="btn btn-primary"
             style="margin-left:10px;margin-right:10px"
             @click="reloadPage">Retake this quiz</button>
            <button class="btn btn-danger" style="margin-left:10px;margin-right:10px">Take another quiz</button>
        </div>
    </div>
</template>

<script>
export default {
    props:['score'],
    data(){
       return{
           showFail:false
       }
    },
    methods:{
        checkIconShown(){
             let score = this.$props.score
             if(score < 50){
                 this.showFail = true
             }
        },
        reloadPage(){
            location.reload();
        }
    },
    mounted(){
        this.checkIconShown()
    }

}
</script>

<style scoped>
 .iconcontainer{
    width: 150px;
    height: 150px;
    /* background: red; */
    margin: auto;
    border-radius: 50%;
    border: 5px solid;
    display: flex;
    justify-content: center;
    align-items: center;
 }

 .failicon{
     border-color:crimson;
 }

 .successicon{
     border-color:forestgreen
 }

 .fa-check , .fa-times{
     font-size:60px;
 }

 .fa-check{
     color:forestgreen;
 }
 .fa-times{
     color:crimson;
 }
</style>

<style>

</style>