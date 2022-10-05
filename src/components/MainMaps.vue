<template>
  <v-tabs v-model="tab">
    <v-tab
        v-for="item in maps"
        :key="item.TabKey">
      {{ item.TabTitle }}
    </v-tab>
  </v-tabs>
  <v-tabs-items v-model="tab">
    <v-tab-item
        v-for="item in maps"
        :key="item.TabKey"
    >
      <component
          :is="item.Component"
          :center="center"
      />
    </v-tab-item>
  </v-tabs-items>
</template>
<script>
import HereBasicMap from "@/components/maps/here.com/HereBasicMap";

export default {
  components: {
    HereBasicMap
  },
  //
  data() {
    return {
      tab: null,
      maps: [
        {
          TabKey: "HereBasicMap",
          TabTitle: "Here Basic Map",
          Component: HereBasicMap,
        }
      ],

      // we are this as prop to the HereMap component
      center: {
        lat: 40.730610,
        lng: -73.935242
      }
    }
  }
}
</script>