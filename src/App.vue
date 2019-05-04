<template>
  <div id="app">
    <Main msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import Main from './components/Main.vue'
import csvtojson from "csvtojson"

const csvFilePath='../public/datasets/country-codes-europe.csv'
const csv=require('csvtojson')
csv()
.fromFile(csvFilePath)
.then((jsonObj)=>{
    console.log(jsonObj);
})

const jsonArray=await csv().fromFile(csvFilePath);

export default {
  name: 'app',
  components: {
    Main
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
