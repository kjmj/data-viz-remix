<template>
  <div>
    <b-navbar :fixed-top="true" :shadow="true">
      <template slot="brand">
        <b-navbar-item v-scroll-to="{ el: '#description', offset: -10 }">
          <img src="../assets/logo.png" alt="Remixing a data visualization" />
        </b-navbar-item>
      </template>
      <template slot="start">
        <b-navbar-item v-scroll-to="{ el: '#oldViz', offset: -10 }">
          Old Viz
        </b-navbar-item>
        <b-navbar-item v-scroll-to="{ el: '#remixedViz', offset: -10 }">
          Remixed Viz
        </b-navbar-item>
      </template>
    </b-navbar>

    <!--Here is our content from index.vue-->
    <div>
      <nuxt />
    </div>

    <footer class="footer">
      <div class="content has-text-centered">
        <p>
          <strong>Remixing a data visualization</strong> by
          <a href="https://github.com/kjmj/" target="_blank">Ken Morton</a> for
          Lane Harrison's Data Visualization course at
          <a href="https://www.wpi.edu/" target="_blank">WPI</a>. Find the
          source code on
          <a href="https://github.com/kjmj/data-viz-remix/" target="_blank"
            >GitHub</a
          >.
        </p>
        <p>
          Made with <a href="https://vuejs.org/" target="_blank">vue.js</a> and
          <a href="https://d3js.org/" target="_blank">d3.js</a>. Here is the
          <a
            href="https://data.world/ansakoy/countries-in-new-york-times-2000-2016/"
            target="_blank"
            >dataset</a
          >.
        </p>
      </div>
    </footer>
  </div>
</template>
