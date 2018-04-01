<template>
  <v-container fluid class='layer-list'>
    <layer v-for='layer in layers' :key='layer.guid' :layer='layer' :clientId='clientId' class='layer layers-section' @setLayerHover='setLayerHover'></layer>
  </v-container>
</template>
<script>
import _ from 'lodash'

import Layer from './ReceiverLayersLayer.vue'

export default {
  name: 'ReceiverLayers',
  props: {
    layers: Array,
    clientId: String
  },
  components: {
    Layer
  },
  computed: {},
  data( ) {
    return {}
  },
  methods: {
    setLayerHover: _.debounce( ( clientId, layerGuid, state ) => {
      Interop.setLayerHover( clientId, layerGuid, state )
    }, 500 )
  },
  mounted( ) {}
}
</script>
<style lang='scss'>
</style>