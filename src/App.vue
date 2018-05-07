<template>
  <div id="app">
    <h1>AutoTrader</h1>
    <car-dimensions />
  </div>
</template>

<script>
import CarDimensions from './components/CarDimensions'
export default {
  name: 'app',
  components: {
    CarDimensions
  },
  data () {
    return {
    
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: "Helvetica Neue",Helvetica,"PingFang SC","Hiragino Sans GB",Arial,sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

</style>
