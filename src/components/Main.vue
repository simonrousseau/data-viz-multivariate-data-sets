<template>
  <div>
    <FilterPanel
      :elevation="elevationData"
      :start="start"
      :end="end"
      :reset="reset"
      :listStation="listStation"
      :stationsEurope="stationsEurope"
      :removeStation="removeStation"
      @update="updateElevation"
      @updateStart="updateStart"
      @updateEnd="updateEnd"
      class="filter-panel"
    />
    <div class="container-fluid zero-padding">
      <MapPanel
        v-if="displayMap === true"
        :filteredStation="filteredStation"
        @update="updateSelectedStation"
        :selectedStation="selectedStation"
        :addStation="addStation"
        :removeStation="removeStation"
        :listStation="listStation"
      />
      <GraphPanel v-else
      :start="start"
      :end="end"
      :listStation="listStation"
      :stationsEurope="stationsEurope"
      :temperatureMonthlyEurope="temperatureMonthlyEurope"
      />
      <DetailPanel
        v-on:displayTemperature="switchDisplay"
        :selectedStation="selectedStation"
        :stationsEurope="stationsEurope"
        :displayMap="displayMap"
        :countryCodeEurope="countryCodeEurope"
      />
    </div>
  </div>
</template>

<script>
import FilterPanel from "./FilterPanel.vue";
import DetailPanel from "./DetailPanel.vue";
import GraphPanel from "./GraphPanel.vue";
import MapPanel from "./MapPanel.vue";

export default {
  name: "Main",
  components: {
    FilterPanel,
    DetailPanel,
    GraphPanel,
    MapPanel
  },
  data: function() {
    return {
      elevationData: [0, 500],
      displayMap: true,
      start: new Date("January 1, 1900 00:00:00"),
      end: new Date(),
      selectedStation: "",
      listStation: []
    };
  },
  methods: {
    switchDisplay() {
      this.displayMap = !this.displayMap;
    },
    updateElevation(newData) {
      this.elevationData = newData;
    },
    updateStart(newData) {
      this.start = newData;
    },
    updateEnd(newData) {
      this.end = newData;
    },
    updateSelectedStation(newData) {
      this.selectedStation = newData;
    },
    reset() {
      this.start = new Date("January 1, 1900 00:00:00");
      this.end = new Date();
      this.elevationData = [0, 500];
      this.selectedStation = ""
      this.listStation = []
      this.displayMap = true;
    },
    addStation(station) {
      if (this.listStation.indexOf(station) === -1) {
        this.listStation.push(station);
      }
    },
    removeStation(station) {
      this.listStation = this.listStation.filter(function(value, index, arr) {
        return value != station;
      });
    }
  },
  props: ["countryCodeEurope", "stationsEurope", "temperatureMonthlyEurope"],
  computed: {
    filteredStation: function() {
      console.log("new date1", this.start.getFullYear());
      return this.stationsEurope.filter(
        elem =>
          elem.Elevation > this.elevationData[0] &&
          elem.Elevation < this.elevationData[1] &&
          Number(elem.YearFirst) > this.start.getFullYear() &&
          Number(elem.YearLast) < this.end.getFullYear()
      );
    }
  }
};
</script>

<style scoped>
.container-fluid {
  display: flex;
  width: 100%;
}
.zero-padding{
  padding: 0px;
}
.filter-panel{
  height: 20vh;
}
</style>
