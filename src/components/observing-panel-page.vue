// Stellarium Web - Copyright (c) 2018 - Noctua Software Ltd
//
// This program is licensed under the terms of the GNU AGPL v3, or
// alternatively under a commercial licence.
//
// The terms of the AGPL v3 license can be found in the main directory of this
// repository.

<template>
<div style="height: 100%">
<v-tabs dark v-model="active" style="height: 0px;">
  <v-tab key="0"></v-tab>
</v-tabs>
<v-tabs-items v-model="active">

  <v-tab-item key="0" style="height: 100%">
    <div style="height: 100%;">
      <v-toolbar dark dense>
        <v-btn icon @click.stop.native="hideObservingPanel"><v-icon>close</v-icon></v-btn>
        <v-spacer></v-spacer>
        {{ userFirstName }}
        <v-menu offset-y left>
          <v-btn icon slot="activator">
            <v-icon>account_circle</v-icon>
          </v-btn>
          <v-list subheader>
            <v-subheader v-if='userLoggedIn'>Logged as {{ userEmail }}</v-subheader>
            <v-list-tile v-if='userLoggedIn' avatar @click="showProfilePage()">
              <v-list-tile-avatar>
                <v-icon>account_circle</v-icon>
              </v-list-tile-avatar>
              <v-list-tile-content>
                <v-list-tile-title>My Profile</v-list-tile-title>
              </v-list-tile-content>
            </v-list-tile>
            <v-list-tile v-if='userLoggedIn' avatar @click="logout()">
              <v-list-tile-avatar>
                <v-icon>exit_to_app</v-icon>
              </v-list-tile-avatar>
              <v-list-tile-content>
                <v-list-tile-title>Logout</v-list-tile-title>
              </v-list-tile-content>
            </v-list-tile>
            <v-list-tile v-if='!userLoggedIn' avatar @click="showLoginPage()">
              <v-list-tile-avatar>
                <v-icon>account_box</v-icon>
              </v-list-tile-avatar>
              <v-list-tile-content>
                <v-list-tile-title>Sign In</v-list-tile-title>
              </v-list-tile-content>
            </v-list-tile>
          </v-list>
        </v-menu>
      </v-toolbar>
      <slot name="main_page"></slot>
    </div>
  </v-tab-item>

  <v-tab-item key="1" style="height: 100%">
    <signin @back="showMainPage()"></signin>
  </v-tab-item>
  
  <v-tab-item key="2" style="height: 100%">
    <my-profile @back="showMainPage()"></my-profile>
  </v-tab-item>

  <slot name="sub_pages"></slot>

</v-tabs-items>
</div>
</template>

<script>
import Signin from './signin.vue'
import MyProfile from './my-profile.vue'
import NoctuaSkyClient from '@/assets/noctuasky-client'

export default {
  data: function () {
    return {
      active: null
    }
  },
  methods: {
    hideObservingPanel: function () {
      this.$store.commit('toggleBool', 'showObservingPanel')
    },
    logout: function () {
      NoctuaSkyClient.users.logout()
    },
    showMainPage: function () {
      this.active = '0'
    },
    showLoginPage: function () {
      this.active = '1'
    },
    showProfilePage: function () {
      this.active = '2'
    }
  },
  computed: {
    userLoggedIn: function () {
      return this.$store.state.plugins.observing.noctuaSky.status === 'loggedIn'
    },
    userFirstName: function () {
      return this.$store.state.plugins.observing.noctuaSky.user.first_name ? this.$store.state.plugins.observing.noctuaSky.user.first_name : 'Anonymous'
    },
    userEmail: function () {
      return this.$store.state.plugins.observing.noctuaSky.user.email
    }
  },
  components: { Signin, MyProfile }
}
</script>

<style>
</style>
