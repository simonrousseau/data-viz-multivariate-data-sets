<template>
  <div id="body">
    <div class="container">
      <div class="elevation">
        <p>Elevation:</p>
        <vue-slider
          :width="200"
          v-model="value"
          :max="500"
          v-on:change="updateValue(value)"
          v-on:drag-end="commitChange()"
        ></vue-slider>
        <span style="margin-right: 150px;">{{elevation[0]}}</span>
        <span>{{elevation[1]}}</span>
      </div>
      <div class="dateStart">
        <p>Date start:</p>
        <datepicker class="datepicker" :value="start" v-on:input="updateLocalStart"></datepicker>
      </div>
      <div class="dateEnd">
        <p>Date end:</p>
        <datepicker class="datepicker" :value="end" v-on:input="updateLocalEnd"></datepicker>
      </div>
      <b-button variant="danger" class="reset" v-on:click="resetLocal">Reset</b-button>
    </div>
    <div class="liste" v-if="listStation.length > 0">
      Selected station(s) :
      <b-button
        class="b-station"
        variant="outline-primary"
        size="sm"
        v-for="station in listStation"
        v-bind:key="station"
        v-on:click="removeStation(station)"
      >
        {{ getName(station) }}
        <i class="fas fa-trash"></i>
      </b-button>
    </div>
  </div>
</template>

<script>
import VueSlider from "vue-slider-component";
import "vue-slider-component/theme/antd.css";
import Datepicker from "vuejs-datepicker";

export default {
  name: "FilterPanel",
  components: {
    VueSlider,
    Datepicker
  },
  data: function() {
    return {
      value: this.elevation,
      local_start: this.start,
      local_end: this.end
    };
  },
  props: [
    "elevation",
    "start",
    "end",
    "reset",
    "listStation",
    "stationsEurope",
    "removeStation"
  ],
  methods: {
    updateValue: function(value) {
      this.value = value;
    },
    updateLocalStart: function(value) {
      this.local_start = value;
      this.$emit("updateStart", this.local_start);
    },
    updateLocalEnd: function(value) {
      this.local_end = value;
      this.$emit("updateEnd", this.local_end);
    },
    commitChange() {
      this.$emit("update", this.value);
    },
    resetLocal() {
      this.reset();
      this.value = [0, 500];
    },
    getName(station) {
      var nameTemp = this.stationsEurope.filter(
        elem => elem.Station == station
      )[0];
      return nameTemp.Name;
    }
  },
  computed: {}
};
</script>

<style scoped>
#body {
  padding-top: 2vh;
  padding-bottom: 1vh;
  background-color: #f2efe9;
  border-bottom: 2px solid rgb(109, 109, 109);
}
.container {
  display: flex;
  width: 100%;
  justify-content: space-around;
  margin-bottom: 1vh;
}
.datepicker {
  z-index: 2000;
}
.reset {
  margin-top: 35px;
  height: 40px;
}
.b-station {
  margin-right: 5px;
  background-color: white;
}
.liste{
  padding-left: 50px;
}
</style>
