<template>
  <div>
    <b-navbar fixed-top="true" shadow="true">
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
  </div>
</template>
