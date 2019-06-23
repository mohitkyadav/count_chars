<template>
  <div class="landing">
    <div id="main-content">

      <v-toolbar color="blue darken-3">
        <v-toolbar-items>
          <v-btn v-for="i in counts" :key="i.type" flat ripple>{{ i.type }} - {{ i.count }}</v-btn>
        </v-toolbar-items>
      </v-toolbar>

      <v-card my-auto>
        <v-layout row>
          <v-flex md12 offset-xs0>
            <v-card class="card--flex-toolbar elevation-16" height="fit-content">
              <v-toolbar color="darken-3" card prominent>
                <v-toolbar-title class="body-2 grey--text"></v-toolbar-title>
                <v-spacer></v-spacer>
                <v-btn icon>
                  <v-icon color="blue darken1">search</v-icon>
                </v-btn>
                <v-btn icon v-on:click="formatCode()">
                  <v-icon color="blue darken1">cached</v-icon>
                </v-btn>
              </v-toolbar>
              <v-divider></v-divider>
              <v-card-text style="height:max-content;">
                <v-text-field
                  id="chars-input"
                  name="input-1"
                  label="Paste your characters here"
                  multi-line
                  v-model="inputText"
                  @keyup="formatCode()"
                  rows="20"
                  :counter="10000000000"
                ></v-text-field>
              </v-card-text>
            </v-card>
          </v-flex>
        </v-layout>
      </v-card>

    </div>

  </div>
</template>

<script>
export default {
  name: 'Landing',
  data () {
    return {
      imgUrl: 'static/BingWallpaper-2018-05-10.jpg',
      drawer: false,
      inputText: '',
      lastStringLen: 0,
      counts: [
        {
          'type': 'Vowels',
          'count': 0
        }, {
          'type': 'Consonents',
          'count': 0
        }, {
          'type': 'Spaces',
          'count': 0
        }, {
          'type': 'Special Chars',
          'count': 0
        }, {
          'type': 'Total',
          'count': 0
        }
      ]
    }
  },
  methods: {
    formatCode: function () {
      this.countChars(this.inputText)
    },
    reFormatCode: function (startIndex) {
      console.log(startIndex)
    },
    countChars: function (text) {
      // console.log(startIndex)
      for (let i = this.lastStringLen; i < text.length; i++) {
        if (text.charCodeAt(i) === 32) {
          this.counts[2].count += 1
        }
        if (text.length > 0) {
          this.lastStringLen = text.length
        } else {
          this.lastStringLen = 0
        }
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#main-content {
  min-height: 100vh;
  flex-direction: column;
}
</style>
