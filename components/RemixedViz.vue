<template>
  <div>
    <h1 class="title">Remixed Viz</h1>
    <h2 class="subtitle">
      The remixed viz will go here
    </h2>
    <p>Country: {{ country }}</p>
    <p># of times mentioned in the NY Times: {{ numTimes }}</p>
    <b-field label="Select a year">
      <b-select placeholder="Select a year" v-model="selectedYear">
        <option v-for="option in years" :value="option" :key="option">
          {{ option }}
        </option>
      </b-select>
    </b-field>
    <svg></svg>
  </div>
</template>

<script>
import * as d3 from 'd3'
import Buefy from 'buefy'
export default {
  name: 'RemixedViz',
  data() {
    return {
      country: '',
      numTimes: '',
      selectedYear: 2016,
      years: [
        2000,
        2001,
        2002,
        2003,
        2004,
        2005,
        2006,
        2007,
        2008,
        2009,
        2010,
        2011,
        2012,
        2013,
        2014,
        2015,
        2016
      ]
    }
  },
  watch: {
    selectedYear: function(newYear, oldYear) {
      this.drawMap()
    }
  },
  mounted() {
    this.drawMap()
  },
  methods: {
    drawMap: function() {
      var width = 960
      var height = 600
      let self = this

      // Add some dimensions to our svg
      var svg = d3
        .select('svg')
        .attr('width', width)
        .attr('height', height)

      // Map and projection
      let projection = d3.geoEqualEarth()

      // Data and color scale
      var data = d3.map()
      var colorScale = d3.scaleQuantize([1, 10000], d3.schemePurples[5])

      // Load external data and boot
      Promise.all([d3.json('world.geojson'), d3.csv('data.csv')]).then(
        ([topo, nytimes]) => {
          setData(nytimes)
          ready(topo)
        }
      )

      // Modify our data to keep track of the number occurrences field from the nytimes data
      function setData(nytimes) {
        for (let i in nytimes) {
          const val = nytimes[i]
          if (val.year == self.selectedYear) {
            data.set(val.country, +val['num occurrences'])
          }
        }
      }

      // Called when the mouse hovers over a country
      let mouseOver = function(d) {
        self.country = d.properties.name
        self.numTimes = d.total
      }

      // Once data is loaded, draw the map
      function ready(topo) {
        svg
          .append('g')
          .selectAll('path')
          .data(topo.features)
          .enter()
          .append('path')
          // draw each country
          .attr('d', d3.geoPath().projection(projection))
          // set the color of each country
          .attr('fill', function(d) {
            d.total = data.get(d.properties.name) || 0
            return colorScale(d.total)
          })
          .on('mouseover', mouseOver)
      }
    }
  },
  components: { Buefy }
}
</script>

<style scoped></style>
