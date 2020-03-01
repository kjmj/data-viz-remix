<template>
  <div>
    <h1 class="title">Remixed Viz</h1>
    <h2 class="subtitle">
      The remixed viz will go here
    </h2>
    <svg></svg>
  </div>
</template>

<script>
import * as d3 from 'd3'
export default {
  name: 'RemixedViz',
  mounted() {
    var width = 960
    var height = 600

    // Add some dimensions to our svg
    var svg = d3
      .select('svg')
      .attr('width', width)
      .attr('height', height)

    // Map and projection
    var projection = d3.geoEqualEarth()

    // Load external data and boot
    d3.json('world.geojson').then(function(data) {
      var gpath = d3.geoPath().projection(projection)

      var color = d3
        .scaleSequential(d3.interpolateBlues)
        .domain(d3.extent(data.features, (d) => gpath.area(d)))

      // Draw the map
      svg
        .append('g')
        .selectAll('path')
        .data(data.features)
        .enter()
        .append('path')
        .attr('fill', (d) => color(gpath.area(d)))
        .attr('d', d3.geoPath().projection(projection))
        .style('stroke', '#fff')
    })

    d3.csv('data.csv').then(function(data) {
      console.log(data)
    })
  }
}
</script>

<style scoped></style>
