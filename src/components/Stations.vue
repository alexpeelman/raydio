<template>
  <v-main class="ma-0 pa-0 pt-2 pl-2">
    <h2 class="pa-0 ma-0 mb-2 primary--text">Stations</h2>
    <v-layout row wrap class="ma-0 pa-0">
      <v-flex v-for="(station, $index) in stations" class="mx-auto mb-2" :key="$index">
        <v-card class="pa-0 ma-0 mr-2" outlined @click="switchStation(station)" elevation="11">
          <div class="d-flex flex-no-wrap justify-space-between">
            <div>
              <v-card-title class="headline"> <country-flag class="mr-0" :country="station.country"></country-flag>{{ station.name }}</v-card-title>
              <v-card-actions>
                <v-chip x-small v-for="(tag, $tagIndex) in station.tags" :key="$tagIndex">{{ tag }}</v-chip>
              </v-card-actions>
            </div>
          </div>
        </v-card>
      </v-flex>
    </v-layout>

  </v-main>
</template>
<script lang="ts">
import Vue from "vue";
import { Component, Emit } from "vue-property-decorator";
import stations from "@/assets/stations.json";
import { Station } from "@/model/Station";
import CountryFlag from "vue-country-flag";

@Component({ components: CountryFlag })
export default class Stations extends Vue {

  private get stations(): Array<Station> {
    return stations;
  }

  @Emit("change:station")
  private switchStation(station: Station): Station {
    return station;
  }
}
</script>