<template>
  <v-container fluid class='layer-list   pa-0'>
    <layer v-for='layer in layers' :key='layer.guid' :layer='layer' :clientId='clientId' class='layer layers-section' @setLayerHover='setLayerHover' @hideLayerHover='hideLayerHover'></layer>
  </v-container>
</template>
<script>
import _ from 'lodash'
import Layer from './SenderLayersLayer.vue'

export default {
  name: 'SenderLayers',
  props: {
    layers: Array,
    objects: Array,
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
    hideLayerHover: _.debounce( ( clientId, layerGuid ) => {
      Interop.setLayerHover( clientId, layerGuid, false )
    }, 250 ),
    setLayerHover: _.debounce( ( clientId, layerGuid ) => {
      Interop.setLayerHover( clientId, layerGuid, true )
    }, 250 )
  },
  mounted( ) {}
}
</script>
<style lang='scss'>
</style>