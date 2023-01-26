<template>
  <main-screen v-if="!endGameStatus && !StatusScreen" @GetTotalSelect = "getTotalFromMain($event)"/>
  <play-screen v-if="!endGameStatus && StatusScreen" 
  :flipElements="settings.flipElements"
  @onPauseGame="endGame($event)"/>
  <result-screen v-if="endGameStatus && StatusScreen"
  :TimeTotal="settings.totalTime"
  @SendReGame="setStatus"/>
</template>
<script>
import {ref} from 'vue';
import MainScreen from './components/MainScreen.vue';
import PlayScreen from './components/PlayScreen.vue';
import {shuffled} from './assets/until/processing.js';
import ResultScreen from './components/ResultScreen.vue';
export default {
  name: 'App',
  components:{
    MainScreen,
    PlayScreen,
    ResultScreen
  },
  
  setup(){
      const settings=ref({
      total:0,
      flipElements:[],
      startAt: 0,
      totalTime: 0
    });
    const StatusScreen = ref(false);
    const endGameStatus = ref(false);
    function setStatus()
    {
      this.StatusScreen = false;
      this.endGameStatus = false;
    }
    function getTotalFromMain(setting)
    {
      this.settings.total = setting.total;
      const firstElements = Array.from(
        {
          length: this.settings.total/2
        },
        (_,i)=>i+1
        );
      const secondElements = [...firstElements];
      const flips = [...firstElements,...secondElements];
      this.settings.flipElements = shuffled(shuffled(shuffled(flips)));
      this.settings.startAt = new Date().getTime();
      this.StatusScreen = true;
    }
    function endGame(status) {
      const endAt = new Date().getTime();
      setTimeout(() => {
        this.settings.totalTime = endAt - this.settings.startAt;
        endGameStatus.value = status
      }, 1500);
    }
    return{
      settings,
      StatusScreen,
      getTotalFromMain,
      endGameStatus,
      endGame,
      setStatus
  }
}
}
</script>
