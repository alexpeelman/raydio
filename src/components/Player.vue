<template>
  <v-main>
    <audio ref="audio">
      <source />
    </audio>
    <v-row class="ma-0 pa-0" align="le">
        <v-col v-if="station" cols="5">
          <v-row>
        <v-subheader v-if="station">{{station.name}}</v-subheader>
        <v-progress-circular v-if="loading" indeterminate />
          </v-row>
        </v-col>
      <v-col>
      <v-btn @click="play" fab small class="ml-1" :disabled="!station">
        <v-icon>mdi-play-circle</v-icon>
      </v-btn>
      <v-btn @click="pause" fab small class="ml-1" :disabled="!station">
        <v-icon>mdi-pause-circle</v-icon>
      </v-btn>
      </v-col>
    </v-row>
  </v-main>
</template>

<script lang="ts">
import Vue from "vue";
import { Component, Prop, Watch } from "vue-property-decorator";
import { Station } from "@/model/Station";

@Component({})
export default class Player extends Vue {

  @Prop({required:false})
  private station:Station | null = null;
  private loading = false;

  get audioComponent(): HTMLAudioElement {
    return (this.$refs.audio as HTMLAudioElement);
  }

  @Watch('station')
  private stationChanged(): void {
    if (this.station) {
      this.audioComponent.src = this.station?.url;
      this.play();
    }
  }

  private play() {
    if (this.station) {
      this.loading = true;
      this.audioComponent.play()
      .finally(r => {
        this.loading = false;
      })
    }
  }

  private pause() {
    if (this.station) {
      this.audioComponent.pause();
    }
  }

}
</script>
