<template>
  <div id="app">
    <h1>Casos de Corona Vírus</h1>
    <div class="corona_box">
      <Box typeBox="state"></Box>  
      <Box typeBox="countrie"></Box>      
    </div>
  </div>
</template>

<script>
import Box from './components/Box.vue'

export default {
  name: 'App',
  components: {
    Box
  }
}
</script>

<style>
body {
    font-family: 'Saira Condensed',sans-serif;
}

#app h1 {
  text-align: center;
  background: linear-gradient(to top right,#ff2a00 0,#ffb600 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.corona_box {
  max-width: 800px;
  margin: 0 auto;
  -webkit-font-moothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 30px;
  display: flex;
  flex-wrap: wrap;
}
</style>