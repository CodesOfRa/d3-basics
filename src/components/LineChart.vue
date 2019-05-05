<template>
  <div>
    <!-- <span>Line Chart</span> -->
    <svg :width="width" :height="heigth">
      <g ref="xAxis" transform="translate(0, 400)"></g>
      <g ref="yAxis" transform="translate(100, 0)"></g>
      <g v-if="data.length === 263">
        <path
          v-for="(d,index) in data"
          :id="d.country"
          :key="d.country"
          :d="generateLine(d.values,index)"
          :stroke="colours(index)"
          stroke-width="1"
          fill="none"
          @mouseover="onClick(d)"
          @click="onClick(d)"
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

      <g v-for="(d,i) in data" :key="d">
        <circle
          v-for="(a,index) in d.values"
          :key="index"
          r="2"
          :fill="colours(i)"
          :stroke="colours(i)"
          :cx="generateCircleCX(a)"
          :cy="generateCircleCY(a)"
          opacity="0.4"
        ></circle>
      </g>
    </svg>
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
  props: ["data", "defaultData"],
  data: function() {
    return { path: d3.line() };
  },
  computed: {
    width: function() {
      return this.defaultData.width + "px";
    },
    heigth: function() {
      return this.defaultData.heigth + "px";
    },
    line: function() {
      this.generateLine();
    },
    colours: function() {
      return d3.scaleSequential(d3.interpolateViridis).domain([0, 265]);
    },
    xScale: function() {
      var xScale = d3
        .scaleLinear()
        .domain([this.defaultData.year.min, this.defaultData.year.max])
        .range([this.defaultData.margin.bottom, this.defaultData.width]);
      return xScale;
    },
    yScale: function() {
      var yScale = d3
        .scaleLinear()
        .domain([0, this.defaultData.value.max])
        .range([
          this.defaultData.heigth - this.defaultData.margin.top,
          this.defaultData.margin.bottom
        ]);
      return yScale;
    }
  },
  mounted() {
    this.$nextTick(
      function() {
        d3.select(this.$refs.xAxis).call(d3.axisBottom(this.xScale).ticks(7));
        d3.select(this.$refs.yAxis).call(d3.axisLeft(this.yScale).ticks(7));
      }.bind(this)
    );
  },
  methods: {
    generateCircleCX: function(data) {
      return this.xScale(data.year);
    },
    generateCircleCY: function(data) {
      return this.yScale(data.value);
    },
    generateLine: function(data) {
      this.path.x(d => this.xScale(d.year)).y(d => this.yScale(d.value));

      return this.path(data);
    },
    onClick: function(e) {
      console.log(e);
    }
  }
};
</script>

