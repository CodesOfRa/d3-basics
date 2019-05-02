<template>
  <div>
    <span>Line Chart</span>
    <svg v-if="data.length === 264">
      <path
        v-for="(datas,index) in data"
        :key="index"
        :d="generateLine(datas.values,index)"
        stroke="pink"
        fill="none"
      ></path>
    </svg>
  </div>
</template>
<script>
const d3 = {
  ...require("d3-array"),
  ...require("d3-shape"),
  ...require("d3-scale")
};
export default {
  props: ["columns", "data", "co2"],
  data: function() {
    return { d: [], lineX: d3.line() };
  },
  computed: {
    line: function() {
      this.generateLine();
    }
  },
  watch: {
    data: function(newVal, oldVal) {
      //   console.log(newVal[0]);
      //   this.generatePath(newVal);
      this.d = newVal[0];
      // watch it
      //   console.log("Prop changed: ", newVal, " | was: ", oldVal);
    },
    line: function() {
      // this.data.map()
      this.generatePath();
    }
  },
  mounted: function() {
    this.generateLine();
    // console.log(this.columns);
    this.$nextTick(function() {
      // DOM is now updated
      // `this` is bound to the current instance
      // this.doSomethingElse()
      //   console.log(this.columns);
    });
  },
  methods: {
    generateLine: function(data, index) {
      console.log("_____", data, index);
      var xScale = d3
        .scaleLinear()
        .domain([1960, 2018]) //input
        .range([0, 400]);
      var yScale = d3
        .scaleLinear()
        .domain([0, 3000])
        .range([30, 0]);
      this.lineX
        .x(
          function(d, i) {
            // console.log(d);
            return xScale(d.value);
          }.bind(this)
        ) // set the x values for the line generator
        .y(
          function(d, i) {
            // console.log(yScale(d[this.columns[i]]));
            // console.log(d);
            return yScale(d.year);
          }.bind(this)
        );
      //   console.log(this.lineX(this.data[0]));
      return this.lineX(Object.keys(data));
      // console.log(this.data);
    }
  }
};
</script>

