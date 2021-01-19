<template>
  <v-main class="ma-0 pa-0 pt-2 pl-2">
    <h2 class="pa-0 ma-0 mb-2 primary--text">Stations</h2>
    <v-text-field
      v-model="search"
      class="mr-2"
      append-icon="mdi-magnify"
      outlined
      placeholder="Search"
    >
    </v-text-field>
    <v-layout row wrap class="ma-0 pa-0">
      <v-flex
        v-for="(station, $index) in stations"
        class="mx-auto mb-2"
        :key="$index"
      >
        <v-card
          class="pa-0 ma-0 mr-2"
          outlined
          @click="switchStation(station)"
          elevation="11"
        >
          <div class="pa-2 text-center">
            <v-card-subtitle class="overline pa-0">
              {{ station.name }}
            </v-card-subtitle>
            <v-avatar v-if="station.img" tile>
              <v-img :src="station.img" contain max-width="200px"></v-img>
            </v-avatar>
          </div>
        </v-card>
      </v-flex>
    </v-layout>
  </v-main>
</template>
<script lang="ts">
import Vue from "vue";
import { Component, Emit, Watch } from "vue-property-decorator";
import stations from "@/assets/stations.json";
import { Station } from "@/model/Station";
import CountryFlag from "vue-country-flag";

@Component({ components: CountryFlag })
export default class Stations extends Vue {
  private allStations: Array<Station> = [];
  private search: string | null = null;

  private created() {
    this.fetchAndMergeRemoteStations();
  }

  @Watch("search")
  private get stations(): Array<Station> {
    return this.allStations
      .sort((a, b) => a.name.localeCompare(b.name))
      .filter(this.matchesSearch);
  }

  private fetchAndMergeRemoteStations(): void {
    const stationNames: Array<string> = stations.map(
      station => station.name
    );
    fetch(`${process.env.VUE_APP_REMOTE_STATIONS_URL}`)
      .then(response => response.json())
      .then((remoteStations: Array<Station>) => {
        return remoteStations.filter(
          r => !stationNames.find(name => name == r.name)
        );
      })
      .then((filteredRemoteStations: Array<Station>) => {
        console.log(filteredRemoteStations)
        this.allStations = [...stations, ...filteredRemoteStations];
      });
  }

  private matchesSearch(station: Station): boolean {
    if (!this.search) {
      return true;
    }

    const nameMatch =
      station.name.toLocaleLowerCase().indexOf(this.search) >= 0;
    const tagMatch =
      station.tags.filter(
        tag => tag.toLocaleLowerCase().indexOf(this.search as string) >= 0
      ).length > 0;

    return nameMatch || tagMatch;
  }

  @Emit("change:station")
  private switchStation(station: Station): Station {
    return station;
  }
}
</script>
