<template>
  <v-app-bar app class="gradient" height="100">
    <v-row class="d-flex align-center" align="center" cl>
      <h4 class="primary--text ">.: <v-icon class="app-icon" large color="primary">mdi-radio</v-icon> :.</h4>
      <h4 class="ml-2 primary--text">Raydio </h4>
      <v-spacer></v-spacer>
      <v-fade-transition v-if="status !== Status.LOADING">
        <v-img v-if="station && station.img" :src="station.img" contain height="80" :aspect-ratio="16/9"></v-img>
        <h1 v-if="station && !station.img" :src="station.img" >{{station.name}}</h1>
      </v-fade-transition>
      <v-spacer></v-spacer>
      <div class="control text-right pr-2">
        <v-progress-circular small v-if="status === Status.LOADING" indeterminate color="primary"/>
          <v-icon large @click="play" color="primary" v-if="status === Status.PAUSED">mdi-play-circle</v-icon>
          <v-icon large @click="pause" color="primary" v-if="status === Status.PLAYING">mdi-pause-circle</v-icon>
        <slot></slot>
      </div>
    </v-row>
    <audio ref="audio">
      <source />
    </audio>
  </v-app-bar>
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
      this.status = Status.LOADING;
      this.audioComponent.src = this.station?.url;
      this.play();
    }
  }

  private play() {
    if (this.station) {
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
  min-width: 80px;
}

.v-toolbar, .v-toolbar__content {
  //height: 108px !important;
}

.gradient {
  background-image: linear-gradient(to right top, rgba(100, 115, 201, 0.33), rgba(25, 32, 72, 0.7));
}
</style>
