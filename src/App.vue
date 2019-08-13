<template>
  <div id="app">
    <v-app :dark='dark'>
      <v-toolbar style='z-index:1;'>
        <v-btn @click='showAccountPopup()' icon small>
          <v-icon class='small' small>account_circle</v-icon>
        </v-btn>
        <v-toolbar-title class="headline text-uppercase mx-0">
          Spe<span @click='showDev()'>ck</span>le
          <span class="font-weight-light">Rhino</span>
        </v-toolbar-title>
        <v-spacer></v-spacer>
        <v-btn color="secondary" dark absolute bottom right fab :ripple="false" @click.native='addReceiver()'>
          <v-icon>cloud_download</v-icon>
        </v-btn>
        <v-btn color="primary" absolute bottom right fab :ripple="false" @click.native='addSender()' style="margin-right:60px">
          <v-icon>cloud_upload</v-icon>
        </v-btn>
      </v-toolbar>
      <v-content>
        <v-container grid-list-md pa-0 mt-4>
          <v-layout row wrap v-if='userAccounts.length===0'>
            <v-flex px-4>
              <v-card color='primary' dark>
                <v-img contain src='https://robohash.org/specklesucks' height='210'></v-img>
                <v-card-text class='text-xs-center'>
                  <b>Howdy, stranger!</b> Seems you have no speckle accounts yet.
                  <v-btn block @click.native='showAccountPopup()'>add an account</v-btn>
                </v-card-text>
              </v-card>
            </v-flex>
          </v-layout>
          <v-layout row wrap>
            <client-manager></client-manager>
          </v-layout>
        </v-container>
      </v-content>
      <register-form></register-form>
      <login-form></login-form>
    </v-app>
  </div>
</template>
<script>
import AccountsManager from './components/AccountsManager.vue'
import ClientManager from './components/ClientManager.vue'
import RegisterForm from './components/RegisterForm.vue'
import LoginForm from './components/LoginForm.vue'

import { EventBus } from './event-bus'

export default {
  name: 'app',
  components: {
    AccountsManager,
    ClientManager,
    RegisterForm,
    LoginForm
  },
  watch: {
    active( newValue ) {
      console.log( newValue )
    }
  },
  computed: {
    userAccounts( ) { return this.$store.getters.accounts.map( a => a.serverName + ', ' + a.email ) },
  },
  data( ) {
    return {
      fab: {},
      active: null,
      dark: false
    }
  },
  methods: {
    showDev( ) {
      Interop.showDev( )
    },
    addReceiver( ) {
      EventBus.$emit( 'show-add-receiver-dialog' )
    },
    addSender( ) {
      EventBus.$emit( 'show-add-sender-dialog' )
    },
    saveClients( ) {
      Interop.saveFileClients( )
    },
    readClients( ) {
      Interop.getFileStreams( )
    },
    showRegistration( ) {
      EventBus.$emit( 'show-register' )
    },
    showLogin( ) {
      EventBus.$emit( 'show-login' )
    },
    purgeClients( ) {
      Interop.removeAllClients( )
        .then( res => {} )
        .catch( res => {} )
    },
    showAccountPopup( ) {
      Interop.showAccountPopup( )
    }
  }
}
</script>
<style>
body {}

.application--dark {
  background-color: #424242 !important;
}

.receiver-content {
  transition: all .3s ease;
}

.receiver-content:last-c hild {
  margin-bottom: 90px;
}

.receiver-content:before {
  content: '\A';
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.03);
  opacity: 1;
  transition: all 0.3s;
  pointer-events: none;
}

.receiver-content:hover:before {
  background: rgba(0, 0, 0, 0);
}

.ellipsis {
  max-width: 100%;
  text-overflow: ellipsis;
  /* Required for text-overflow to do anything */
  white-space: nowrap;
  overflow: hidden;
}

.xs-actions {
  font-size: 16px !important;
}

.layer-list {
  padding: 0;
}

.layer {
  padding: 2px 0 2px 0;
  transition: all .3s ease;
}

.layer:last-child {}

.layers-section {
  position: relative;
  transition: all 0.3s;
  border-top: 1px dashed grey;
}

.layers-section:last-child {
  border-bottom: 1px dashed grey;
}

.layers-section:hover:after {
  opacity: 0;
}

.layers-section:after {
  content: '\A';
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.07);
  opacity: 1;
  transition: all 0.3s;
  pointer-events: none;
}
</style>