<template>
  <div class="body">
    <l-map :zoom="zoom" :center="center">
      <l-tile-layer :url="url"></l-tile-layer>
      <l-marker
        v-for="station in filteredStation"
        :lat-lng="[station.Latitude, station.Longitude]"
        v-bind:key="station.Station"
        v-on:click="markerClicked(station.Station)"
        :icon="iconDisplay(station.Station)"
      ></l-marker>
    </l-map>
  </div>
</template>

<script>
import { LMap, LTileLayer, LMarker } from "vue2-leaflet";
import { stat } from "fs";

export default {
  name: "LeafletMap",
  props: [
    "filteredStation",
    "selectedStation",
    "addStation",
    "removeStation",
    "listStation"
  ],
  components: {
    LMap,
    LTileLayer,
    LMarker
  },
  methods: {
    markerClicked(id) {
      if (this.listStation.includes(id)) {
        this.removeStation(id);
      } else {
        this.addStation(id);
      }

      this.$emit("update", id);
    },
    iconDisplay: function(id) {
      if (this.listStation.includes(id)) {
        return L.icon({
          iconUrl: "https://image.flaticon.com/icons/svg/660/660623.svg",
          iconSize: [32, 32] // size of the icon
        });
      } else {
        return L.icon({
          iconUrl: "https://image.flaticon.com/icons/svg/149/149060.svg",
          iconSize: [32, 32] // size of the icon
        });
      }
    }
  },
  data() {
    return {
      zoom: 4,
      center: L.latLng(50, 6),
      url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png"
    };
  },
  computed: {}
};
</script>

<style scoped>
.body {
  width: 80%;
  height: 80vh;
}
</style>
