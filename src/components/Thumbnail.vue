<template>
  <div class="thumbnail">
    <svg id="map"></svg>
  </div>
</template>

<script>
import * as d3 from "d3";

export default {
  name: "Thumbnail",
  data() {
    return {
      loadData: {}
    };
  },
  mounted() {
    //    this.fetchData();
    const map = d3.select("#map");
    console.dir(window.innerWidth);
    map
      .append("circle")
      .attr("cx", 200)
      .attr("cy", 200)
      .attr("r", 80)
      .style("fill", "rgba(0, 0, 0, 0.8)");
  },
  methods: {
    async fetchData() {
      let data = await d3.json("./data.json");
      this.loadData = data;
    }
  }
};
</script>

<style scoped>
.thumbnail,
#map {
  margin: 0;
  width: 100%;
  height: 100%;
}
</style>
