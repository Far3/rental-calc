<template>
  <div id="app">
    <div class="cos-contain">
      <!-- <img alt="Vue logo" src="./assets/COS.jpg"> -->
      <rentalCalc/>
    </div>
  </div>
</template>

<script>
import rentalCalc from './components/rentalCalc.vue'

export default {
  name: 'App',
  components: {
    rentalCalc
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
img {
  width: 200px;
  height: auto;
}
</style>
