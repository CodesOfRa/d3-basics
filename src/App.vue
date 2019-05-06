<template>
  <div id="app" class="wrapper">
    <h1>CO2 emissions</h1>
    <line-chart
      class="center"
      :defaultData="defaultData"
      :data="data"
      :selected="selected"
      @onHover="handleHover"
    ></line-chart>
    <div>
      <div class="legendDetails">
        <span>Legend</span>
        <span v-if="hover">{{pickedCountry}}</span>
      </div>

      <div class="legendT">
        <div
          :class="['legendA',!hover?'hoverActive':'']"
          v-for="(c,index) in country"
          :key="country[index]"
          @mouseover="handleHover(index)"
        >
          <div class="legend" :style="{ background:colours(index)}"/>
        </div>
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
        width: 1210,
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
      hover: false,
      pickedCountry: "",
      selected: new Array(263).fill(1)
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
    },
    // handleHover: function(index) {
    //   console.log(index);
    //   this.selected.fill(1);
    //   this.selected[index] = 8;
    // },
    handleHover: function(index) {
      // version1
      this.selected.fill(1);
      this.selected[index] = 10;
      this.selected = [...this.selected];
      this.hover = true;
      this.pickedCountry = this.country[index];
      // version 2
      /* const newSelected = [...this.selected];
      newSelected[index] = 10;
      this.selected = newSelected;*/

      // this.selected[index] = this.selected[index] + 1;

      //version 3
      // this.$set(this.selected, index, this.selected[index] + 1);
      console.log(this.selected[index]);
    }
  }

  // mouseOver:
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
  display: flex;
}
.legend {
  height: 20px;
  width: 6px;
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
.legendDetails span {
  margin: 1rem;
}
.legendA span {
  font-size: 9px;
  display: inline-block;
  white-space: nowrap;
  transform: translate(0, 100%) rotate(-90deg);
  transform-origin: 0 0;
  vertical-align: bottom;
}
.center {
  text-align: center;
}
.wrapper h1,
line-chart {
  /* display: flex; */
  text-align: center;
}
.hoverActive {
  border: 2px solid red;
}
</style>
