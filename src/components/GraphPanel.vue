<template>
  <div class="body">
    <div class="info-bloc">
      <button
        v-if="monthDisplay"
        type="button"
        class="btn btn-primary"
        @click="back()"
      ><i class="fas fa-chevron-left"></i> Back to full year</button>
      <span v-if="monthDisplay">Selected year : {{selectedYear}}</span>
    </div>

    <Chart v-if="!monthDisplay" class="chart" :chartdata="chartdata" :options="options"/>
    <ChartMonth v-else class="chart" :chartdata="chartdataMonth" :options="optionsMonth"/>
  </div>
</template>

<script>
import Chart from "./Chart.vue";
import ChartMonth from "./ChartMonth.vue";

export default {
  name: "GraphPanel",
  components: { Chart, ChartMonth },
  data() {
    return {
      monthDisplay: false,
      selectedYear: 0
    };
  },
  props: [
    "start",
    "end",
    "listStation",
    "temperatureMonthlyEurope",
    "stationsEurope"
  ],
  computed: {
    computedDate: function() {
      var labelTemp = [];
      for (
        let index = 0;
        index <= this.end.getFullYear() - this.start.getFullYear();
        index++
      ) {
        labelTemp[index] = this.start.getFullYear() + index;
      }
      return labelTemp;
    },
    chartdata: function() {
      var datasetsTemp = this.listStation.map(station => {
        var nameTemp = this.stationsEurope.filter(
          sta => sta.Station == station
        )[0];

        var dataTemp = this.temperatureMonthlyEurope.filter(temp => {
          return (
            temp.Station == station &&
            temp.Year > this.start.getFullYear() &&
            temp.Year < this.end.getFullYear()
          );
        });

        dataTemp = dataTemp.map(temp => {
          return {
            x: new Date(temp.Year, temp.Month),
            y: temp.Temperature
          };
        });

        return {
          label: nameTemp["Name"],
          borderColor: "#" + ((Math.random() * 0xffffff) << 0).toString(16),
          data: dataTemp
        };
      });

      return {
        labels: this.computedDate,
        datasets: datasetsTemp
      };
    },
    chartdataMonth: function() {
      var datasetsTemp2 = this.listStation.map(station => {
        var nameTemp = this.stationsEurope.filter(
          sta => sta.Station == station
        )[0];

        var dataTemp2 = this.temperatureMonthlyEurope.filter(temp => {
          return temp.Station == station && temp.Year == this.selectedYear;
        });

        dataTemp2 = dataTemp2.map(temp => {
          return {
            x: temp.Month,
            y: temp.Temperature
          };
        });

        return {
          label: nameTemp["Name"],
          borderColor: "#" + ((Math.random() * 0xffffff) << 0).toString(16),
          data: dataTemp2
        };
      });

      return {
        labels: [
          "jan",
          "feb",
          "mar",
          "apr",
          "may",
          "jun",
          "jul",
          "aug",
          "sep",
          "oct",
          "nov",
          "dec"
        ],
        datasets: datasetsTemp2
      };
    },
    options: function() {
      var _this = this;
      return {
        responsive: true,
        pan: {
          enabled: true,
          mode: "xy"
        },
        zoom: {
          enabled: true,
          mode: "xy"
        },
        maintainAspectRatio: false,
        onClick: function(evt) {
          _this.displayMonthView(evt, this);
        },
        tooltips: {
          mode: "index",
          intersect: false
        },
        scales: {
          xAxes: [
            {
              type: "time",
              display: true,
              time: {
                displayFormats: {
                  quarter: "YYYY"
                }
              },
              scaleLabel: {
                display: true,
                labelString: "Date"
              }
            }
          ],
          yAxes: [
            {
              display: true,
              scaleLabel: {
                display: true,
                labelString: "Temperature (°C)"
              }
            }
          ]
        }
      };
    },
    optionsMonth: function() {
      var _this = this;
      return {
        responsive: true,
        pan: {
          enabled: true,
          mode: "xy"
        },
        zoom: {
          enabled: true,
          mode: "xy"
        },
        maintainAspectRatio: false,
        tooltips: {
          mode: "index",
          intersect: false
        },
        scales: {
          xAxes: [
            {
              display: true,
              scaleLabel: {
                display: true,
                labelString: "Date"
              }
            }
          ],
          yAxes: [
            {
              display: true,
              scaleLabel: {
                display: true,
                labelString: "Temperature (°C)"
              }
            }
          ]
        }
      };
    }
  },
  methods: {
    getRandomInt() {
      return Math.floor(Math.random() * (50 - 5 + 1)) + 5;
    },
    displayMonthView(e, ctx) {
      if (e.layerY > 340) {
        var xLabel = ctx.scales["x-axis-0"].getValueForPixel(e.x);
        console.log(xLabel.format("YYYY"));
        this.monthDisplay = true;
        this.selectedYear = xLabel.format("YYYY");
      }
    },
    back() {
      console.log("Back");
      this.monthDisplay = false;
      this.selectedYear = 0;
    }
  }
};
</script>

<style scoped>
.body {
  width: 80%;
  height: 80vh;
  background-color: rgb(240, 240, 240);
}
.chart {
  margin-top: 40px;
}
.info-bloc{
  margin: 5px;
}
</style>
