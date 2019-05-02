<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <line-chart :columns="columns" :data="data" :co2="co2"></line-chart>
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import LineChart from "./components/LineChart.vue";
// import data from "../data.csv";
import * as d3 from "d3";
// const d3 = { ...require("d3") };
export default {
  name: "app",
  components: {
    HelloWorld,
    LineChart
  },
  data() {
    return {
      data: [],
      columns: []
    };
  },
  mounted: function() {
    this.getData();
  },
  methods: {
    async getData() {
      let data = await d3.csv("../co2.csv");
      // console.log(data[0]);
      data.columns.map(d => {
        if (!isNaN(d)) {
          this.columns.push(d);
          //convert strings to numbers
          data.map(
            data => (data[d] = +data[d])
            // console.log("_______" + data["Country Name"] + "-----" + data[d])
          );
        }
      });
      var co2 = [];
      data.map(item => {
        let country = { country: item["Country Name"], values: [] };
        Object.keys(item).map(key => {
          if (!isNaN(key)) {
            // console.log(key);
            let aux = { year: key, value: item[key] };
            country.values.push(aux);
          }
        });
        co2.push(country);
        // console.log(Object.keys(item));
      });
      console.log(co2);
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
