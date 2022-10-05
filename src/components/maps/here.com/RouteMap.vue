<template>
  <div id="map">
    <!--In the following div the HERE Map will render-->
    <div id="mapContainer" style="height:600px;width:100%" ref="hereMap"></div>
  </div>
</template>

<script>

export default {
  name: "RouteMap",
  components: {},
  props: {
    center: Object
    // center object { lat: 40.730610, lng: -73.935242 }
  },
  data() {
    return {
      platform: null,
      apikey: process.env.VUE_APP_HERE_API_KEY
      // You can get the API KEY from developer.here.com
    };
  },
  async mounted() {
    // Initialize the platform object:
    const platform = new window.H.service.Platform({
      apikey: this.apikey
    });
    this.platform = platform;


    // https://developer.here.com/documentation/examples/maps-js


    this.$nextTick(() => {
      this.$nextTick(() => {
        this.initializeHereMap();
      });
    });

    // this.$nextTick(() => {
    //   return this.$nextTick()
    // }).finally(() => {
    //   this.initializeHereMap();
    // })

    // this.mNextTick(2, () => {
    //   this.initializeHereMap();
    // });
  },
  methods: {
    /*mNextTick(nrOfTicks, fn) {
      let prevTick = null;
      for (let i = 1; i <= nrOfTicks; i++) {
        if (i === 1) { // first one
          prevTick = () => {
            this.$nextTick(fn);
          }
        } else {
          // add more!
          if (i === nrOfTicks) {
            // Execute it!
            this.$nextTick(prevTick);
          } else {
            prevTick = () => {
              this.$nextTick(prevTick);
            }
          }
        }
      }
    },*/
    svgMarker(text) {
      return '<svg width="24" height="24" ' +
          'xmlns="http://www.w3.org/2000/svg">' +
          '<rect stroke="white" fill="#1b468d" x="1" y="1" width="22" ' +
          'height="22" /><text x="12" y="18" font-size="12pt" ' +
          'font-family="Arial" font-weight="bold" text-anchor="middle" ' +
          'fill="white">' + text + '</text></svg>';
    },

    initializeHereMap() { // rendering map
// Retrieve the target element for the map:
//       var targetElement = document.getElementById('mapContainer');
      const mapContainer = this.$refs.hereMap;
      const H = window.H;

// Get the default map types from the platform object:

      var maptypes = this.platform.createDefaultLayers();

      // Instantiate (and display) a map object:
      var map = new H.Map(mapContainer, maptypes.vector.normal.map, {
        zoom: 10,
        center: this.center
        // center object { lat: 40.730610, lng: -73.935242 }
      });

      // Create the parameters for the routing request:
      var routingParameters = {
        'routingMode': 'fast',
        'transportMode': 'truck',
        // The start point of the route:
        // 'origin': '50.1120423728813,8.68340740740811',
        'origin': '40.712598095317205,-74.00584860194301',
        // The end point of the route:
        // 'destination': '52.5309916298853,13.3846220493377',
        'destination': '34.05223904982168,-118.24365264224112',
        // Include the route shape in the response
        'return': 'polyline'
        // https://developers.google.com/maps/documentation/utilities/polylineutility decode polyline?!
      };

      // https://github.com/heremaps/flexible-polyline
      addEventListener("resize", () => map.getViewPort().resize());


      // add behavior control
      new H.mapevents.Behavior(new H.mapevents.MapEvents(map));

      // add UI
      H.ui.UI.createDefault(map, maptypes);
      // End rendering the initial map


// Define a callback function to process the routing response:
      var onResult = (result) => {
        // ensure that at least one route was found
        if (result.routes.length) {
          result.routes[0].sections.forEach((section) => {
            // Create a linestring to use as a point source for the route line
            let linestring = H.geo.LineString.fromFlexiblePolyline(section.polyline);

            // Create a polyline to display the route:
            let routeLine = new H.map.Polyline(linestring, {
              style: {strokeColor: 'blue', lineWidth: 3}
            });


// Create an icon, an object holding the latitude and longitude, and a marker:
            let startMarker = new H.map.Marker(
                section.departure.place.location,
                {icon: new H.map.Icon(this.svgMarker("P1"))}
            );
            // Create a marker for the start point:
            // let startMarker = new H.map.Marker(section.departure.place.location);

            // Create a marker for the end point:
            // let endMarker = new H.map.Marker(section.arrival.place.location);
            let endMarker = new H.map.Marker(
                section.arrival.place.location,
                {icon: new H.map.Icon(this.svgMarker("D1"))}
            );

            // Add the route polyline and the two markers to the map:
            map.addObjects([routeLine, startMarker, endMarker]);

            // Set the map's viewport to make the whole route visible:
            map.getViewModel().setLookAtData({bounds: routeLine.getBoundingBox()});


            //Within the onResult callback:

// Create an outline for the route polyline:
            var routeOutline = new H.map.Polyline(linestring, {
              style: {
                lineWidth: 10,
                strokeColor: 'rgba(0, 128, 255, 0.7)',
                lineTailCap: 'arrow-tail',
                lineHeadCap: 'arrow-head'
              }
            });
// Create a patterned polyline:
            var routeArrows = new H.map.Polyline(linestring, {
                  style: {
                    lineWidth: 10,
                    fillColor: 'white',
                    strokeColor: 'rgba(255, 255, 255, 1)',
                    lineDash: [0, 2],
                    lineTailCap: 'arrow-tail',
                    lineHeadCap: 'arrow-head'
                  }
                }
            );
// create a group that represents the route line and contains
// outline and the pattern
//             routeLine.addObjects([routeOutline, routeArrows]);
            map.addObjects([routeLine, routeOutline, routeArrows]);
          });
        }
      };

// Get an instance of the routing service version 8:
      var router = this.platform.getRoutingService(null, 8);

      // https://developer.here.com/documentation/maps/3.1.35.0/api_reference/H.service.RoutingService8.html
// Call calculateRoute() with the routing parameters,
// the callback and an error callback function (called if a
// communication error occurs):
      router.calculateRoute(
          routingParameters,
          onResult,
          function (error) {
            console.log("calculate route error", error);
            alert(error.message);
          }
      );
    }
  }
};
</script>

<style scoped>
#map {
  /*width: 60vw;*/
  min-width: 360px;
  min-height: 360px;
  text-align: center;
  /*margin: 5% auto;*/
  background-color: #ccc;
}
</style>