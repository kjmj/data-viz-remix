<template>
  <div>
    <h1 class="title">Remixed Viz</h1>
    <h2 class="subtitle">
      The remixed viz will go here
    </h2>
    <b-field label="Select a year">
      <b-select placeholder="Select a year" v-model="selectedYear">
        <option v-for="option in years" :value="option" :key="option">
          {{ option }}
        </option>
      </b-select>
    </b-field>
    <svg id="map"></svg>
    <body>
      <div id="heatmap"></div>
    </body>
  </div>
</template>

<script>
import * as d3 from 'd3'
import Buefy from 'buefy'
export default {
  name: 'RemixedViz',
  data() {
    return {
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
      ],
      nyDataPromise: null,
      topoDataPromise: null,
      tooltip: d3
        .select('body')
        .append('div')
        .style('position', 'absolute')
        .style('z-index', '10')
        .style('visibility', 'hidden')
        .style('background-color', 'white')
        .style('border', 'solid')
        .style('border-width', '2px')
        .style('border-radius', '5px')
        .style('padding', '5px')
    }
  },
  mixins: [
    {
      methods: {
        mouseover: function(d, vm) {
          this.tooltip.style('visibility', 'visible')

          d3.select(vm)
            .style('stroke', 'black')
            .style('opacity', 1)
        },
        mouseout: function(d, vm) {
          this.tooltip.style('visibility', 'hidden')

          d3.select(vm)
            .style('stroke', 'none')
            .style('opacity', 0.8)
        },
        mousemove: function(d, country, numOccurrences, year) {
          this.tooltip
            .html(
              'Country: ' +
                country +
                '<br>' +
                'Year: ' +
              numOccurrences +
                '<br>' +
                'Number of occurences: ' +
                year
            )
            .style('top', event.pageY - 10 + 'px')
            .style('left', event.pageX + 10 + 'px')
        }
      }
    }
  ],
  watch: {
    selectedYear: function(newYear, oldYear) {
      this.drawMap()
    }
  },
  created() {
    // load in ny dataset
    this.nyDataPromise = new Promise(function(resolve, reject) {
      d3.csv('ny_times_data.csv')
        .then(function(data) {
          resolve(data)
        })
        .catch(function(e) {
          reject(Error(e))
        })
    })

    // load in topo dataset
    this.topoDataPromise = new Promise(function(resolve, reject) {
      d3.json('world.geojson')
        .then(function(data) {
          resolve(data)
        })
        .catch(function(e) {
          reject(Error(e))
        })
    })
  },
  mounted() {
    this.drawMap()
    this.drawHeatMap()
  },
  methods: {
    drawMap: function() {
      let vm = this

      var width = 940
      var height = 600

      // Add some dimensions to our svg
      var svg = d3
        .select('#map')
        .attr('width', width)
        .attr('height', height)

      // Map and projection
      let projection = d3.geoEqualEarth()

      // Data and color scale
      var data = d3.map()
      var colorScale = d3.scaleQuantize([1, 10000], d3.schemePurples[5])

      // Load external data and boot
      Promise.all([vm.topoDataPromise, vm.nyDataPromise]).then(
        ([topo, nytimes]) => {
          setData(nytimes)
          ready(topo)
        }
      )

      // Modify our data to keep track of the number occurrences field from the nytimes data
      function setData(nytimes) {
        for (let i in nytimes) {
          const val = nytimes[i]
          if (val.year == vm.selectedYear) {
            data.set(val.country, +val.numOccurrences)
          }
        }
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
            d.total = data.get(d.properties.name) || 'No data'
            return colorScale(d.total)
          })
          .on('mouseover', function(d) {
            vm.mouseover(d, this)
          })
          .on('mousemove', function(d) {
            vm.mousemove(d, d.properties.name, vm.selectedYear, d.total)
          })
          .on('mouseout', function(d) {
            vm.mouseout(d, this)
          })
      }
    },
    drawHeatMap: function() {
      let vm = this

      // set the dimensions and margins of the graph
      var margin = { top: 30, right: 30, bottom: 30, left: 225 },
        width = 1000 - margin.left - margin.right,
        height = 3000 - margin.top - margin.bottom

      // append the svg object to the body of the page
      var svg = d3
        .select('#heatmap')
        .append('svg')
        .attr('width', width + margin.left + margin.right)
        .attr('height', height + margin.top + margin.bottom)
        .append('g')
        .attr('transform', 'translate(' + margin.left + ',' + margin.top + ')')

      //Read the data
      vm.nyDataPromise.then(function(data) {
        // Sort data by country
        data.sort((a, b) =>
          a.country > b.country ? -1 : a.country < b.country ? 1 : 0
        )

        // Labels of row and columns
        var years = []
        var countries = []
        data.forEach(function(d) {
          years.push(d.year)
          countries.push(d.country)
        })

        // Remove duplicates
        years = [...new Set(years)]
        countries = [...new Set(countries)]

        // Build X scales and axis:
        var x = d3
          .scaleBand()
          .range([0, width])
          .domain(years)
          .padding(0.01)

        // top axis label
        svg.append('g').call(d3.axisTop(x))

        svg
          .append('g')
          .attr('transform', 'translate(0,' + height + ')')
          .call(d3.axisBottom(x))

        // Build X scales and axis:
        var y = d3
          .scaleBand()
          .range([height, 0])
          .domain(countries)
          .padding(0.01)

        // left axis label
        svg.append('g').call(d3.axisLeft(y))

        // Build color scale
        var colorScale = d3.scaleQuantize([1, 10000], d3.schemePurples[5])

        // Create the colormap
        svg
          .selectAll()
          .data(data, function(d) {
            return d.year + ':' + d.country
          })
          .enter()
          .append('rect')
          .attr('x', function(d) {
            return x(d.year)
          })
          .attr('y', function(d) {
            return y(d.country)
          })
          .attr('width', x.bandwidth())
          .attr('height', y.bandwidth())
          .style('fill', function(d) {
            return colorScale(d.numOccurrences)
          })
          .on('mouseover', function(d) {
            vm.mouseover(d, this)
          })
          .on('mousemove', function(d) {
            vm.mousemove(d, d.country, d.numOccurrences, d.year)
          })
          .on('mouseout', function(d) {
            vm.mouseout(d, this)
          })
      })
    }
  },
  components: { Buefy }
}
</script>

<style scoped></style>
