<template>
  <div>
    <Description id="description"></Description>
    <OldViz id="oldViz"></OldViz>
    <RemixedViz id="remixedViz"></RemixedViz>

    <!--Placeholder so we can test scroll-to-->
    <div style="height: 3000px"></div>
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
