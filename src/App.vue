<template>
  <transition-group name="slide-container" mode="out-in">
    <component
      key="mainComponent" 
      @activeComponentEvent="activeComponent = $event"
      :is="activeComponent">
    </component>
    <canvas key="canvas" id="canvas" v-show="activeComponent == 'app-celebrate'"></canvas>
  </transition-group>
</template>

<script>

import CardVue from './components/Card.vue'
import GameCardVue from './components/GameCard.vue'
import DefaultCardVue from './components/DefaultCard.vue'
import CelebrateVue from './components/Celebrate.vue'
import FailureVue from './components/Failure.vue'

export default {
  components: {
    appCard: CardVue,
    appGameCard: GameCardVue,
    appDefaultCard: DefaultCardVue,
    appCelebrate: CelebrateVue,
    appFailure: FailureVue
  },
  data(){
    return {
      activeComponent: "appGameCard"
    }
  }
}
</script>

<style>

body {
  font-family: sans-serif;
}

/* .slide-container-enter{} */
.slide-container-enter-active{
  animation: slide-in 0.2s ease-out forwards;
}
/* .slide-container-leave{} */
.slide-container-leave-active{
  animation: slide-out 0.2s ease-out forwards;  
}

@keyframes slide-in {
  from {
    transform: translateX(-1000px);
    opacity: 0;
  }
  to {
    transform: translateX(0px);
    opacity: 1;
  }
}

@keyframes slide-out {
  from {
    transform: translateX(0px);
    opacity: 1;
  }
  to {
    transform: translateX(1000px);
    opacity: 0;
  }
}

</style>
