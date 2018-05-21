<template>
  <div class="landing">
    <v-card flat>
      <v-toolbar dark color="blue" flat extended>
        <v-toolbar-side-icon></v-toolbar-side-icon>
      </v-toolbar>
      <v-layout row pb-2>
        <v-flex xs8 offset-xs2>
          <v-card class="card--flex-toolbar">
            <v-toolbar color="grey darken-4" card prominent>
              <v-toolbar-title class="body-2 grey--text">Paste your text here</v-toolbar-title>
              <v-spacer></v-spacer>
              <v-btn icon>
                <v-icon>search</v-icon>
              </v-btn>
            </v-toolbar>
            <v-divider></v-divider>
            <v-card-text style="height: 200px;"></v-card-text>
          </v-card>
        </v-flex>
      </v-layout>
    </v-card>
  </div>
</template>

<script>
export default {
  name: 'Landing',
  data () {
    return {
      imgUrl: 'static/BingWallpaper-2018-05-10.jpg',
      drawer: false
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
