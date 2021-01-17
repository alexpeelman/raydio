<template>
  <v-main class="ma-2">
    <h2>Stations</h2>
    <v-card v-for="(station, $index) in stations" class="mx-auto mb-2" :key="$index">
      <v-list-item>
        <v-card-subtitle>{{ station.name }}</v-card-subtitle>
        <v-btn small @click="play(station)">
          <v-icon >mdi-play-box</v-icon>
        </v-btn>
      </v-list-item>
    </v-card>
  </v-main>
</template>
<script lang="ts">
import Vue from "vue";
import { Component, Emit } from "vue-property-decorator";
import stations from "@/assets/stations.json";
import { Station } from "@/model/Station";

@Component({})
export default class Stations extends Vue {

  private get stations(): Array<Stations> {
    return stations;
  }

  @Emit("change:station")
  private play(station: Station): Station {
    return station;
  }
}
</script>