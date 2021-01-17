<template>
  <v-app>
    <v-app-bar app dark>
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <h2>Rudi-O</h2>
      </div>

      <v-spacer></v-spacer>

      <v-icon @click="closeApp">mdi-close</v-icon>
    </v-app-bar>

    <v-main>
      <Player :station="currentStation"/>
      <Stations @change:station="onChangeStation" />
    </v-main>
  </v-app>
</template>

<script lang="ts">
import Vue from "vue";
import Player from "./components/Player.vue";
import { Component } from "vue-property-decorator";
import Stations from "@/components/Stations.vue";
import { Station } from "@/model/Station";

@Component({
  components: {
    Stations,
    Player
  }
})
export default class Rudio extends Vue {

  private station: Station | null = null;

  private closeApp() {
    window.top.close();
  }

  private get currentStation(): Station {
    return this.station;
  }

  private onChangeStation(station: Station) {
    this.station = station;
  }
}
</script>
<style>
html {
  overflow-y: hidden;
}
</style>
