<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <!--  <HelloWorld msg="Welcome to Your Vue.js App"/>-->
  <HereMap :center="center"/>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import HereMap from './components/HereMap.vue'

export default {
  name: 'App',
  components: {
    // HelloWorld
    HereMap
  },
  //
  data() {
    return {
      // we are this as prop to the HereMap component
      center: {
        lat: 40.730610,
        lng: -73.935242
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
