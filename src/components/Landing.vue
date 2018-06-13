<template>
  <div class="landing">
    <v-card flat>
      <v-toolbar dark color="blue" class="elevation-3" flat>
        <v-toolbar-side-icon></v-toolbar-side-icon>
      </v-toolbar>
      <v-layout row pb-2>
        <v-flex xs10 offset-xs1>
          <v-card class="card--flex-toolbar elevation-16">
            <v-toolbar color="grey darken-3" card prominent>
              <v-toolbar-title class="body-2 grey--text"></v-toolbar-title>
              <v-spacer></v-spacer>
              <v-btn icon>
                <v-icon color="blue darken1">search</v-icon>
              </v-btn>
            </v-toolbar>
            <v-divider></v-divider>
            <v-card-text style="height:400px;">
              <v-text-field
                id="chars-input"
                name="input-1"
                label="Paste your chars here"
                multi-line="true"
                :counter="10000000000"
              ></v-text-field>
            </v-card-text>
          </v-card>
        </v-flex>
      </v-layout>
    </v-card>
    <v-footer height="auto" fixed>
      <v-card flat tile class="flex">
        <v-card-text class="blue">
          <strong class="subheading text-md-center"></strong>
          <v-spacer></v-spacer>
          <v-btn
            v-for="icon in icons"
            :key="icon"
            icon
            dark
            class="mx-3"
          >
            <v-icon size="24px">{{ icon }}</v-icon>
          </v-btn>
        </v-card-text>
        <v-card-actions class="grey lighten-2 justify-center">
          &copy;2018 — <strong>Mohit Kumar Yadav</strong>
        </v-card-actions>
      </v-card>
    </v-footer>
  </div>
</template>

<script>
export default {
  name: 'Landing',
  data () {
    return {
      imgUrl: 'static/BingWallpaper-2018-05-10.jpg',
      drawer: false,
      icons: ['fab fa-facebook', 'fab fa-twitter', 'fab fa-google-plus', 'fab fa-linkedin', 'fab fa-instagram'],
      rows: [
        {
          title: 'Company Name',
          children: ['Here you can use rows and columns here to organize your footer content. Lorem ipsum dolor sit amet, consectetur adipisicing elit']
        },
        {
          title: 'Products',
          children: ['MDBootstrap', 'MDWordPress', 'BrandFlow', 'Bootstrap Angular']
        },
        {
          title: 'Useful Links',
          children: ['Your account', 'Become an Affiliate', 'Shipping Rates', 'Helper']
        }
      ]
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
