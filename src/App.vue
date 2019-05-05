<template>
  <div id="app">
    <line-chart :defaultData="defaultData" :data="data" :selected="selected" @onHover="handleHover"></line-chart>
    <span>Legend</span>
    <div class="legendT">
      <div class="legendA" v-for="(c,index) in country" :key="country[index]">
        <div class="legend" :style="{ background:colours(index), heigth:'50px'}" width="50px"/>
        <span>{{country[index]}}</span>
      </div>
    </div>
  </div>
</template>

<script>
import LineChart from "./components/LineChart.vue";
import * as d3 from "d3";

export default {
  name: "app",
  components: {
    LineChart
  },
  data() {
    return {
      data: [],
      country: [],
      defaultData: {
        width: 1200,
        heigth: 500,
        margin: {
          top: 100,
          bottom: 100
        },
        year: {
          min: 1960,
          max: 2014
        },
        value: {
          max: 38000000
        }
      },
      selected: new Array(262).fill(1)
    };
  },
  computed: {
    colours: function() {
      console.log("----");
      return d3.scaleSequential(d3.interpolateViridis).domain([0, 265]);
    }
  },
  mounted: function() {
    this.getData();
  },
  methods: {
    async getData() {
      let data = await d3.csv("../co2.csv");
      var co2 = [];
      data.map(item => {
        let country = { country: item["Country Name"], values: [] };
        this.country.push(item["Country Name"]);
        Object.keys(item).map(key => {
          if (!isNaN(key)) {
            //convert strings to numbers
            item[key] = +item[key];
            let aux = { year: key, value: item[key] };
            country.values.push(aux);
          }
        });
        co2.push(country);
      });

      this.data = co2;
    }
  },
  handleHover: function(index) {
    console.log(index);
    this.selected.fill(1);
    this.selected[index] = 2;
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
}
.legendT {
  /* display: flex; */
}
.legend {
  height: 10px;
  width: 10px;
}
.legendA {
  display: flex;
  margin: 0;
  padding: 0;
}
.legend > div {
  display: flex;
  justify-content: center;
  flex-direction: column;
}
.legendA span {
  /* transform: rotate(-90deg);
  transform: rotate(-90deg);
  font-size: 9px; */
  font-size: 9px;
}
</style>
