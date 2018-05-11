<template>
  <div id="app">
    <div class="container">
      <h1>AutoTrader</h1>      
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
$c-font: #5e6d82;
$c-font-strong: #1f2f3d;

#app {
  font-family: Helvetica Neue,Helvetica,PingFang SC,Hiragino Sans GB,Microsoft YaHei,SimSun,sans-serif;
  font-weight: 400;
  -webkit-font-smoothing: antialiased;
  color: $c-font;  
  h1, h2 {
    font-weight: 400;
    color: $c-font-strong;
  }
}
.container {
  max-width: 1000px;
  margin: auto;
}
</style>
