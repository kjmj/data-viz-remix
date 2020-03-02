<template>
  <div>
    <section class="section" id="description">
      <div class="container">
        <Description></Description>
      </div>
    </section>

    <section class="section" id="oldViz">
      <div class="container">
        <OldViz></OldViz>
      </div>
    </section>
    <no-ssr>
      <section class="section" id="remixedViz">
        <div class="container">
          <RemixedViz></RemixedViz>
        </div>
      </section>
    </no-ssr>
  </div>
</template>

<script>
import OldViz from '../components/OldViz'
import Description from '../components/Description'
import RemixedViz from '../components/RemixedViz'

export default {
  name: 'HomePage',

  components: {
    RemixedViz,
    OldViz,
    Description
  }
}
</script>
