<template>

<div class="timer">

    <h1 class="timer__title">
      {{ hours }}:{{ minutes }}:{{ seconds }} -
      {{ timer.details.timezone }}
    </h1>
    <button @click="deleteZone" class="timer__button btn btn-danger">DELETE</button>
    
</div>
    

</template>

<script>
import timeDetails from "@/mixins/timeDetails.js";
export default {
      mixins: [timeDetails],
    name:"Timer",
    data(){
return{
    zone:this.timerZone
}
    },
    props:{
        timer:Object,
        timerZone:String,
        indexOfZone:Number
    },
    mounted(){
        setInterval(this.changeTime,1000);
    },
    methods:{
        changeTime(){
            this.getTimeDetails().then((timeDetails) => {
                    this.timer.seconds = timeDetails.seconds;
                     this.timer.minutes = timeDetails.minutes;
                      this.timer.hours = timeDetails.hours;
    });
    },
       deleteZone(){
        this.$emit("delete",this.indexOfZone)
        },
        
    },
    computed:{
    hours() {
      return this.timer.hours
    },
    minutes() {
      return this.timer.minutes
    },
    seconds() {
      return this.timer.seconds
    },
    },
}
</script>

<style>


</style>
