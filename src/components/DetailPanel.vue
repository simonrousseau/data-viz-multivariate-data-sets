<template>
  <div class="body">
    <p class="no-selected" v-if="!selectedStation">No station selected</p>
    <div v-else class="details">
      <h5>Details :</h5><br/>
      <p>CountryCode: <span class="badge badge-light"> {{stationInformation.CountryCode}}</span></p>
      <p>Country: <span class="badge badge-light"> {{getCountry(stationInformation.CountryCode).Name}}</span></p>
      <p>Elevation: <span class="badge badge-light"> {{stationInformation.Elevation}}</span></p>
      <p>Latitude: <span class="badge badge-light"> {{stationInformation.Latitude}}</span></p>
      <p>Longitude: <span class="badge badge-light"> {{stationInformation.Longitude}}</span></p>
      <p>Name: <span class="badge badge-light"> {{stationInformation.Name}}</span></p>
      <p>Station: <span class="badge badge-light"> {{stationInformation.Station}}</span></p>
      <p>YearFirst: <span class="badge badge-light"> {{stationInformation.YearFirst}}</span></p>
      <p>YearLast: <span class="badge badge-light"> {{stationInformation.YearLast}}</span></p>
      <button v-on:click="$emit('displayTemperature')" type="button" class="btn btn-primary" v-if="displayMap">Compare temperature readings</button>
      <button v-on:click="$emit('displayTemperature')" type="button" class="btn btn-primary" v-else>Back to map</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "DetailPanel",
  components: {},
  props: ["countryCodeEurope","displayTemperature","selectedStation","stationsEurope","displayMap"],
  computed: {
    stationInformation: function() {
      return this.stationsEurope.filter(
        elem => elem.Station === this.selectedStation
      )[0];
    },
  },
  methods: {
    getCountry(cc){
      return this.countryCodeEurope.filter(
        elem => elem.Code === cc
      )[0];
    }
  }
};
</script>

<style scoped>
.body {
  background-color: #f2efe9;
  width: 20%;
  border-left: 2px solid rgb(109, 109, 109);
}
.details{
  margin: 20px;
}
.no-selected{
  margin-top: 100px;
  text-align: center;
}
</style>
