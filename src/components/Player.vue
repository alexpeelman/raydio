<template>
  <v-main class="ma-0 pa-0 pl-2 pr-2">
    <v-card elevation="8" outlined class="mt-4">
      <v-expand-transition>
        <v-card-title class="headline" v-if="station">{{ station.name }}
          <span class="ml-1">
          <v-progress-circular v-if="status === Status.LOADING" indeterminate />
          <v-btn @click="play" fab small class="ml-1" v-if="status === Status.PAUSED">
            <v-icon>mdi-play-circle</v-icon>
          </v-btn>
          <v-btn @click="pause" color="secondary" small fab v-if="status === Status.PLAYING">
            <v-icon>mdi-pause-circle</v-icon>
          </v-btn>
          </span>
        </v-card-title>
      </v-expand-transition>
    </v-card>
    <audio ref="audio">
      <source />
    </audio>
  </v-main>
</template>

<script lang="ts">
import Vue from "vue";
import { Component, Prop, Watch } from "vue-property-decorator";
import { Station } from "@/model/Station";

enum Status {
  NONE,
  PAUSED,
  PLAYING,
  LOADING,
  FAILED
}

@Component({})
export default class Player extends Vue {

  @Prop({ required: false })
  private station: Station | null = null;
  private status: Status = Status.NONE;
  private Status = Status;

  get audioComponent(): HTMLAudioElement {
    return (this.$refs.audio as HTMLAudioElement);
  }

  @Watch("station")
  private stationChanged(): void {
    if (this.station) {
      this.audioComponent.src = this.station?.url;
      this.play();
    }
  }

  private play() {
    if (this.station) {
      this.status = Status.LOADING;
      this.audioComponent
        .play()
        .catch((error) => {
          this.status = Status.FAILED;
        })
        .finally(() => {
          this.status = Status.PLAYING;
        });
    }
  }

  private pause() {
    if (this.station) {
      this.audioComponent.pause();
      this.status = Status.PAUSED;
    }
  }
}

</script>
