<template>
  <MainMaps/>
</template>

<script>
import MainMaps from "@/components/MainMaps";

export default {
  name: 'App',
  components: {
    MainMaps
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
