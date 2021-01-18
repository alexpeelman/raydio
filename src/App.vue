<template>
  <v-app>
    <v-app-bar app>
      <v-row class="d-flex align-center" align="center" cl>
        <h2 class="primary--text ">.: <v-icon class="app-icon" large color="primary">mdi-radio</v-icon> :.</h2>
        <h2 class="ml-2 primary--text">Rudi-O </h2>
      </v-row>

      <v-spacer></v-spacer>

      <v-icon large color="primary" @click="closeApp">mdi-close</v-icon>
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

  private get currentStation(): Station | null{
    return this.station;
  }

  private onChangeStation(station: Station) {
    this.station = station;
  }
}
</script>
<style lang="scss">
html {
  overflow-y: hidden;
}

.app-icon {
  position: relative;
  top: -5px;
}
</style>
