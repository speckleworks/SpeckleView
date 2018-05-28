<template>
  <div>
    <receiver-add></receiver-add>
    <sender-add></sender-add>
    <v-slide-y-transition>
      <v-card fluid fill-height class='pa-4 elevation-0' v-show='glLoading'>
        Loading...
        <v-progress-linear height='1' :indeterminate='true'></v-progress-linear>
      </v-card>
    </v-slide-y-transition>
    <v-card fluid fill-height v-if='clients.length == 0 && !glLoading' class='elevation-0 pa-4'>
      <h4>Hey there!</h4>
      <p>There are no clients in this file. You can add some from the big button in the lower right corner!</p>
    </v-card>
    <v-container fluid v-if='clients.length > 0' style='min-height: 100%;' class='pa-0'>
      <template v-for='client in clients'>
        <component :is='getClientType( client.Role )' :client='client'></component>
        <!-- <rhino-receiver v-if='client.Role === 1' :client='client'></rhino-receiver>
        <rhino-sender v-else :client='client'></rhino-sender> -->
      </template>
    </v-container>
  </div>
</template>
<script>
import ReceiverAdd from './ClientReceiverAdd.vue'
import SenderAdd from './ClientSenderAdd.vue'

import RhinoReceiver from './RhinoReceiver.vue'
import RhinoSender from './RhinoSender.vue'
import RevitReceiver from './RevitReceiver.vue'
import RevitSender from './RevitSender.vue'


import { EventBus } from '../event-bus'

export default {
  name: 'AccountsManager',
  components: {
    ReceiverAdd,
    SenderAdd,
    RhinoReceiver, RhinoSender,
    RevitReceiver, RevitSender
  },
  computed: {
    clients( ) {
      return this.$store.getters.clients
    },
    glLoading( ) {
      return this.$store.state.globalLoading
    },
    hostApp( ) {
      return this.$store.state.hostApplication
    },
    receiverComponent( ) {

    },
    senderComponent( ) { 
    }
  },
  data( ) {
    return {
      addClientDialog: false
    }
  },
  methods: {
    getClientType( role ) {
      switch( role ) {
        case 1: 
          return this.hostApp.toLowerCase( ) + '-receiver'
        break
        default:
          return this.hostApp.toLowerCase( ) + '-sender'
        break
      }
    }
  },
  mounted( ) {}
}
</script>
<style scoped>
.receiver:last-child {
  margin-bottom: 100px;
}
</style>