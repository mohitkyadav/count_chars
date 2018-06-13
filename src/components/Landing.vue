<template>
  <div class="landing">
    <v-card flat>
      <v-toolbar dark color="blue" class="elevation-3" flat>
        <v-toolbar-side-icon></v-toolbar-side-icon>
      </v-toolbar>
      <v-layout row pb-2>
        <v-flex xs10 offset-xs1>
          <v-card class="card--flex-toolbar elevation-16" height="fit-content">
            <v-toolbar color="grey darken-3" card prominent>
              <v-toolbar-title class="body-2 grey--text"></v-toolbar-title>
              <v-spacer></v-spacer>
              <v-btn icon>
                <v-icon color="blue darken1">search</v-icon>
              </v-btn>
               <v-btn v-on:click="formatCode()">
                <v-icon color="blue darken1">cached</v-icon>
              </v-btn>
            </v-toolbar>
            <v-divider></v-divider>
            <v-card-text style="height:max-content;">
              <v-text-field
                id="chars-input"
                name="input-1"
                label="Paste your chars here"
                multi-line
                :counter="10000000000"
              ></v-text-field>
            </v-card-text>
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
  },
  methods: {
    formatCode: function () {
      console.log('Hi, You just clicked a button.')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
