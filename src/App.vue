<template>
  <v-app>
    <v-main>
      <MainMaps/>
    </v-main>
  </v-app>
</template>

<script>
import MainMaps from './components/MainMaps.vue'

export default {
  name: 'App',

  components: {
    MainMaps,
  },

  data: () => ({
    //
  }),
}
</script>
