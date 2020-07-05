<template>
  <div class="thumbnail">
    <svg id="map"></svg>
  </div>
</template>

<script>
import * as d3 from "d3";
import * as topojson from "topojson";
import * as polyline from "@mapbox/polyline";

export default {
  name: "Thumbnail",
  data() {
    return {
      loadData: {}
    };
  },
  mounted() {
    const map = d3.select("#map");

    const projection = d3
      .geoMercator()
      .center([145.6, 31.6])
      .scale(5000)
      .translate([window.innerWidth, window.innerHeight]);
    const path = d3.geoPath().projection(projection);

    // 一時的に拝借 https://github.com/miyawa-tarou/japan_map_json/blob/master/data/topojson/pref.jso
    d3.json("./static/pref.json").then(function(data) {
      map
        .selectAll("path")
        .data(topojson.feature(data, data.objects.pref).features)
        .enter() // dataで入れた配列をそれぞれ以下のpathにしていく
        .append("path")
        .attr("d", path) // <path d="">にpath()の返り値を入れていく
        .attr("fill", "#ddd3be")
        .attr("stroke", "#ff346a")
        .attr("stroke-opacity", 0.3)
        .attr("stroke-dasharray", 3);

      const line =
        new URL(window.location).searchParams.get("line") ||
        "_`ovEkptaYjrAr`CvgBxkCbqCj~@fnCrcB|~CfaAnaG~OriCf{BdmAfeDtpAe@lPx~Blh@tsEnzBrfA~KryD|lBtu@xx@``CtdCka@l}Dci@hjClUdhCh]tqBxiAf`B`dAnxBfFlbDt`@~_CrNfr@tnBpLxl@r}@op@`oBpf@`cApPtzCqaBxaH_lBraKdoBnuIbd@llHeeB`iCqm@fdCzxBvdBbvBv_AbcA|rBrpAfpE}~@bzChFkp@nXwgEwr@ekEnAdmAlnAtzE_~@l{Cid@x`Cyo@dZxSxNRr{o@jh@ojAt}Bi@b{I{LztCgU|~DgdBpr@wcBpRmyBlt@udAzgAolBzJwgBhFmr@dcEafBj`Dw|Cd_A__GucAirDcy@_uBgbA_jD__DmmFgkJg}BunFqEibL_@cmL_oBq_Dqi@qoDsZkfDufClBuvCkj@qpCii@wiB_}A{{Ba_@o|CmaAafCzl@_oDndA{_Cwr@gdDw_AgxDqp@g|CquA}lBiBm~B}Ia_FysAm|DglDa[caGsSw{C}mBer@_lCobBoo@_cDkz@emByrB";
      const geojson = polyline.toGeoJSON(line);

      map
        .datum(geojson)
        .append("path")
        .attr("d", path)
        .attr("stroke", "deeppink")
        .attr("stroke-opacity", 0.8)
        .attr("stroke-width", 3)
        .attr("fill", "none");

      // TODO : fix fitsize
      const widthPadding = window.innerWidth * 0.2;
      const heightPadding = window.innerHeight * 0.1;
      projection.fitExtent(
        [
          [widthPadding / 2, heightPadding / 2],
          [window.innerWidth - widthPadding, window.innerHeight - heightPadding]
        ],
        geojson
      );
      const t0 = projection.translate();
      const s0 = projection.scale();
      const interpolateTranslate = d3.interpolate(t0, projection.translate());
      const interpolateScale = d3.interpolate(s0, projection.scale());
      const interpolator = function(t) {
        projection
          .scale(interpolateScale(t))
          .translate(interpolateTranslate(t));
        map.selectAll("path").attr("d", path);
      };

      d3.transition()
        .duration(0)
        .tween("projection", function() {
          return interpolator;
        });
    });
  }
};
</script>

<style scoped>
.thumbnail,
#map {
  margin: 0;
  width: 100%;
  height: 100%;
  background: #03c6e2;
}
</style>
