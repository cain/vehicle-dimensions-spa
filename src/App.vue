<template>
  <div id="app">
    <!-- <h1>AutoTrader</h1> -->
    <div class="container">
      <dimensions-search />
    </div>
  </div>
</template>

<script>
import DimensionsSearch from './components/DimensionsSearch'
export default {
  name: 'app',
  components: {
    DimensionsSearch
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
.container {
  max-width: 1000px;
  margin: auto;
}
</style>
