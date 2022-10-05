<template>
  <v-card>
    <v-toolbar
        color="primary">
      <v-toolbar-title>Maps</v-toolbar-title>
    </v-toolbar>
    <v-tabs
        centered=""
        v-model="tab"
        color="primary">
      <v-tab :key="map.TabTitle" v-for="map in maps">
        <v-icon start>
          mdi-account
        </v-icon>
        {{ map.TabTitle }}
      </v-tab>
    </v-tabs>
    <v-window v-model="tab">
      <v-window-item :key="map.TabTitle" v-for="(map, i) in maps" :value="i">
        <v-card flat>
          <component :is="map.Component" :center="center"/>
        </v-card>
      </v-window-item>
    </v-window>
  </v-card>
</template>
<script>
import HereBasicMap from "@/components/maps/here.com/BasicMap";
import HereRouteMap from "@/components/maps/here.com/RouteMap";

export default {
  components: {},
  //
  data() {
    return {
      tab: 0,
      maps: [
        {
          TabKey: "HereBasicMap",
          TabTitle: "Here Basic Map",
          Component: HereBasicMap,
        },
        {
          TabKey: "HereRouteMap",
          TabTitle: "Here Route Map",
          Component: HereRouteMap,
        }
      ],

      // we are this as prop to the HereMap component
      center: {
        lat: 40.730610,
        lng: -73.935242
      }
    }
  },

  mounted() {

  }
}
</script>