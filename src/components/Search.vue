<template>
  <v-container>
    <div>
      <h1 class="heading grey--text">{{ title }}</h1> 
    </div>
    <div>
      <v-form>
        <div>
          <v-row>
            <v-col cols="12" md="4">
              <v-text-field
                v-model="term"
                outlined
                dense
                filled
                label="Search"
              ></v-text-field>
            </v-col>
            <v-col cols="12" md="4">
              <v-btn rounded @click="search()">
                <v-icon dark large>mdi-magnify</v-icon>
              </v-btn>
            </v-col>
          </v-row>
          <v-row>
            <v-btn @click="sortAlphabeticA_Z()">
              <v-icon dark large>mdi-sort-alphabetical-descending-variant</v-icon>
            </v-btn>
            <v-btn @click="sortAlphabeticZ_A()">
              <v-icon dark large>mdi-sort-alphabetical-ascending-variant</v-icon>
            </v-btn>
          </v-row>
        </div>
      </v-form>
      <div>
       <v-row>
        <v-col cols="2" v-for="result in results"
          :key="result.id">
          <v-card>
            <v-img 
              :src="result.artworkUrl100">
            </v-img>
            <v-card-title>
              {{ result.trackName }}
            </v-card-title>

            <v-card-subtitle>
              {{ result.artistName }}
            </v-card-subtitle>

            <v-card-subtitle>
              {{ result.collectionName }}
            </v-card-subtitle>

            <v-card-subtitle>
              {{ millisToMinutesAndSeconds(result.trackTimeMillis) }}
            </v-card-subtitle>
            
            <v-card-subtitle>
              {{  result.currency + result.collectionPrice}}
            </v-card-subtitle>


          </v-card>    
        </v-col>
      </v-row>    
      </div>
    </div>
  </v-container>
</template>

<script>
import axios from "axios";
  export default {
    name: 'Search',

    data: () => ({
      title: "iTunes",
      results: [],
      term: null,
      entity:"song",
      media:"music",
      itemsPerPage: 20,
    }),
    methods: {
    async search() {
      await axios
        .get(
          `https://itunes.apple.com/search?term=${this.term}&media=${this.media}&entity=${this.entity}&limit=200`
        )
        .then((res) => {
          console.log(res);
          console.log("res: " + res.data.results[0].artistName);
          this.results = res.data.results;
        });
    },
    sortAlphabeticA_Z() {
      this.results.sort((a, b) => (a.trackName > b.trackName ? 1 : -1));
    },
    sortAlphabeticZ_A() {
      this.results.sort((a, b) => (a.trackName < b.trackName ? 1 : -1));
    },
    millisToMinutesAndSeconds(duration) {
    var seconds = parseInt((duration/1000)%60)
    , minutes = parseInt((duration/(1000*60))%60)
    minutes = (minutes < 10) ? "0" + minutes : minutes;
    seconds = (seconds < 10) ? "0" + seconds : seconds;

    return  minutes + ":" + seconds;
    }
  },
};
</script>
