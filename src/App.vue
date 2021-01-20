<template>
  <v-app>
    <Player ref="player" :station="station">
      <v-icon large class="pl-2" fab color="primary" v-if="appMode" @click="closeApp">mdi-close-circle</v-icon>
    </Player>
    <v-main>
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
export default class Raydio extends Vue {

  private station: Station | null = null;

  private created() {
    window.addEventListener('keypress', this.togglePlay);
  }

  private destroyed() {
    window.removeEventListener('keypress', this.togglePlay);
  }

  private get appMode(): boolean {
    return (process.env.IS_ELECTRON || false) as boolean;
  }

  private closeApp() {
    window.top.close();
  }

  private onChangeStation(station: Station) {
    this.station = station;
  }

  public togglePlay(e: KeyboardEvent):void {
    if (e.code == "Space") {
      (this.$refs.player as Player).togglePlay();
    }
  }
}
</script>
<style lang="scss">
html {
  background: #121212;

  /* Hide scrollbar for Chrome, Safari and Opera */
  ::-webkit-scrollbar {
    display: none;
  }

  /* Hide scrollbar for IE, Edge and Firefox */
    -ms-overflow-style: none;  /* IE and Edge */
    scrollbar-width: none;  /* Firefox */
}

.app-icon {
  position: relative;
  top: -5px;
}
</style>
