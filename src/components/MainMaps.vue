<template>
  <v-tabs>
    <v-tab>
      <HereBasicMap :center="center"/>
    </v-tab>
    <v-tab>Item Two</v-tab>
    <v-tab>Item Three</v-tab>
  </v-tabs>
</template>
<script>
import HereBasicMap from "@/components/maps/HereBasicMap";

export default {
  components: {
    HereBasicMap
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