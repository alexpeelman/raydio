<template>
  <v-main class="ma-0 pa-0 pl-2 pr-2">
    <v-expand-transition>
      <v-card elevation="8" outlined class="mt-4 gradient" >
        <v-row class="elevation-12 ma-0 pa-0 text-center" align="center">
          <v-img v-if="station" :src="station.img" contain height="100" :aspect-ratio="16/9"></v-img>
          <div class="control pl-2">
            <v-progress-circular v-if="status === Status.LOADING" indeterminate />
            <v-btn @click="play" color="secondary" large fab v-if="status === Status.PAUSED">
              <v-icon>mdi-play-circle</v-icon>
            </v-btn>
            <v-btn @click="pause" color="secondary" large fab v-if="status === Status.PLAYING">
              <v-icon>mdi-pause-circle</v-icon>
            </v-btn>
          </div>
        </v-row>
      </v-card>
    </v-expand-transition>
    <audio ref="audio">
      <source />
    </audio>
  </v-main>
</template>

<script lang="ts">
import Vue from "vue";
import { Component, Prop, Watch } from "vue-property-decorator";
import { Station } from "@/model/Station";
import { VSwitch } from "vuetify/lib";

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

  public togglePlay() {
    if (this.status == Status.PAUSED) {
      this.play();
    } else if (this.status == Status.PLAYING) {
      this.pause();
    }
  }
}
</script>
<style lang="scss">
.control {
  position: fixed;
}

.gradient {
  background-image: linear-gradient(to right top, rgba(100, 115, 201, 0.33), rgba(25, 32, 72, 0.7));
}
</style>
