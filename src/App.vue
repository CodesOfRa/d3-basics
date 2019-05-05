<template>
  <div id="app">
    <line-chart :defaultData="defaultData" :data="data"></line-chart>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
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
      }
    };
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
        Object.keys(item).map(key => {
          if (!isNaN(key)) {
            item[key] = +item[key];
            let aux = { year: key, value: item[key] };
            country.values.push(aux);
          }
        });
        co2.push(country);
        // console.log(Object.keys(item));
      });

      this.data = co2;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
