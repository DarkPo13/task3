<template>
  <div id="app">
    <h1 class="title">World time application</h1>
    
    <zones @update-time="updateTime" @add-time="addTime" />

    <div class="alert">

    <h2 id="alert__header_limit" > ONLY 5 TIMEZONES, MY FRIEND! </h2>
    <h2 id="alert__header_equality" > ALREADY EXIST </h2>
    
    </div>

    <timer v-for="(time, index) in times" :key="index" :timer="time" :timerZone="time.details.timezone" :indexOfZone="index" @delete="deleteZone"/>

  </div>
</template>

<script>
var counter = 1;
import Zones from "./components/Zones.vue";
import timeDetails from "@/mixins/timeDetails.js";
import Timer from "./components/Timer.vue"

export default {
  mixins: [timeDetails],
  name: "App",
  components: {
    Zones,Timer
  },
  data() {
    return {
      times: [],
      zone: "Europe/Kiev",
    };
  },
  mounted() {
    this.getTimeDetails().then((timeDetails) => {
      this.addTime(timeDetails);
    });
    setInterval(this.setTime, 1000);
  },
  methods: {
    
        changeTime(timer){
          const addSeconds = timer.date.getSeconds() + 1;
          const newTime = timer.date.setSeconds(addSeconds);
          timer.date = new Date(newTime);
          return timer;
        },

        setTime(){
          this.times= this.times.map(time=>this.changeTime(time));
        },

    isExist(time){
      let check = false;
      this.times.forEach(timeInArray => {
        if(time.details.timezone==timeInArray.details.timezone){
          check = true;
        }
      });
      return check;
    },

    deleteZone(zoneId){
     this.times.splice(zoneId,1);
    },

    updateTime(timeDetails) {
      if(!this.isExist(timeDetails)){
      if (!this.times.length) {
        this.addTime(timeDetails);
        return;
      }

      this.times.splice(this.times.length - 1, 1, timeDetails);
      }else{
        document.getElementById("alert__header_equality").style.display="block";
      }
    },
    addTime(time) {
      
      if(!this.isExist(time)){
        document.getElementById("alert__header_limit").style.display="none";
        document.getElementById("alert__header_equality").style.display="none";
      if(counter<=5){
      this.times.push(time);
      console.log(counter);
      counter++;
      }else{
        document.getElementById("alert__header_limit").style.display="block";
      }
      }else{
        document.getElementById("alert__header_equality").style.display="block";
      }
    },

  },
};
</script>

<style lang="scss">
#app {
  text-align: center;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

.title {
  text-align: center;
}

.default-el {
  border: 1px solid #75c5b7;
  padding: 10px;
  cursor: pointer;
  outline: none;
  transition: all 0.2s;
  font-weight: 600;
  border-radius: 2px;
  color: #75c5b7;
  background: #f8fff6;
}

.btn {
  background-color: #75c5b7;
  color: #fff;

  &:active {
    background-color: #fff;
  }
}

.ml-1 {
  margin-left: 0.2rem;
}

.alert{
  text-align: center;
  color: rgb(153, 20, 20);
}

#alert__header_limit,#alert__header_equality{
  display: none;
  }
</style>
