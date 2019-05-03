<template>
  <div>
    <!-- <span>Line Chart</span> -->
    <svg width="1200px" height="500px">
      <g ref="xAxis" transform="translate(0, 400)"></g>
      <g ref="yAxis" transform="translate(100, 0)"></g>
      <g v-if="data.length === 263">
        <path
          v-for="(datas,index) in data"
          :id="datas.country"
          :key="datas.country"
          :d="generateLine(datas.values,index)"
          :stroke="colours(index)"
          stroke-width="1"
          fill="none"
          @mouseover="onClick(datas)"
          @click="onClick(datas)"
        ></path>
        <!-- <circle
          v-for="(a,i) in datas.values"
          :key="i"
          r="2"
          :fill="colours(index)"
          :stroke="colours(index)"
          :cx="generateCircleCX(a)"
          :cy="generateCircleCY(a)"
          opacity="0.4"
        ></circle>-->
      </g>

      <!-- <g v-for="(d,i) in data" :key="d">
        <circle
          v-for="(a,index) in d.values"
          :key="index"
          r="2"
          :fill="colours(i)"
          :stroke="colours(i)"
          :cx="generateCircleCX(a)"
          :cy="generateCircleCY(a)"
          opacity="0.4c"
        ></circle>
      </g>-->
    </svg>
    <svg width="1200px"></svg>
  </div>
</template>
<script>
const d3 = {
  ...require("d3-array"),
  ...require("d3-shape"),
  ...require("d3-scale"),
  ...require("d3-scale-chromatic"),
  ...require("d3-axis"),
  ...require("d3-selection")
};
export default {
  props: ["columns", "data", "co2"],
  data: function() {
    return { d: [], lineX: d3.line() };
  },
  computed: {
    line: function() {
      this.generateLine();
    },
    colours: function() {
      //   console.log(index);
      return d3.scaleSequential(d3.interpolateViridis).domain([0, 265]);
    }
  },
  mounted() {
    this.$nextTick(
      function() {
        //   this.width = this.$refs.lineChart.offsetWidth;
        //   this.height = this.$refs[this.id].offsetHeight;
        //   this.drawChart();
        var xScale = d3
          .scaleLinear()
          .domain([1960, 2014]) //input
          .range([100, 1200]);
        var yScale = d3
          .scaleLinear()
          .domain([0, 38000000])
          .range([400, 100]);
        console.log(this.$refs);
        d3.select(this.$refs.xAxis).call(d3.axisBottom(xScale).ticks(7));
        d3.select(this.$refs.yAxis).call(d3.axisLeft(yScale).ticks(7));
      }.bind(this)
    );
  },
  methods: {
    generateCircleCX: function(data) {
      //   console.log(data);
      var xScale = d3
        .scaleLinear()
        .domain([1960, 2014]) //input
        .range([100, 1200]);
      return xScale(data.year);
    },
    generateCircleCY: function(data) {
      var yScale = d3
        .scaleLinear()
        .domain([0, 38000000])
        .range([400, 100]);
      return yScale(data.value);
    },
    generateLine: function(data, index) {
      // console.log("_____", data, index);
      var xScale = d3
        .scaleLinear()
        .domain([1960, 2014]) //input
        .range([100, 1200]);
      var yScale = d3
        .scaleLinear()
        .domain([0, 38000000])
        .range([400, 100]);
      this.lineX
        .x(function(d, i) {
          return xScale(d.year);
        }) // set the x values for the line generator
        .y(function(d, i) {
          return yScale(d.value);
        });

      return this.lineX(data);
    },
    onClick: function(e) {
      console.log(e);
    },
    generateColour: function(index) {
      console.log(index);
      //   if (index !== this.data.length) {
      return colours[index];
      //   }
    }
  }
};
</script>

