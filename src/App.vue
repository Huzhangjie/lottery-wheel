<template>
  <div id="app">
    <Lottery 
      lottery-bg="./imgs/turnplate-bg.png"
      pointer-bg="./imgs/pointer.png"
      :lottery-start="lotteryStart"
      @lotteryClick="lotteryClick"
    />
  </div>
</template>

<script>
import Lottery from './components/Lottery.vue'

export default {
  name: 'App',
  components: {
    Lottery
  },
  data() {
    return {
      lotteryStart: 0
    }
  },
  methods: {
    lotteryClick() {
      console.log("lotteryClick -> lotteryClick")
    }
  }
}
</script>

<style>
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
</style>
