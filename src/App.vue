<template>
  <div id="app">
    <Main
      :countryCodeEurope="countryCodeEurope"
      :stationsEurope="stationsEurope"
      :temperatureMonthlyEurope="temperatureMonthlyEurope"
    />
  </div>
</template>

<script>
import * as d3 from "d3";
import Main from "./components/Main.vue";

export default {
  name: "app",
  components: {
    Main
  },
  data: function() {
    return {
      countryCodeEurope: [],
      stationsEurope: [],
      temperatureMonthlyEurope: []
    };
  },
  mounted() {
    console.log("App loaded");
    this.fetchData();
  },
  methods: {
    async fetchData() {
      let countryCodeEurope = await d3.csv(
        "./datasets/country-codes-europe.csv"
      );
      let stationsEurope = await d3.csv("./datasets/stations-europe.csv");
      let temperatureMonthlyEurope = await d3.csv(
        "./datasets/temperature-monthly-europe.csv"
      );

      this.countryCodeEurope = countryCodeEurope;
      this.stationsEurope = stationsEurope;
      this.temperatureMonthlyEurope = temperatureMonthlyEurope;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
html,
body, h3 {
  margin: 0;
  padding: 0;
}
</style>
