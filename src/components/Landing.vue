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
      inputText: '',
      vowASCII: [65, 97, 69, 101, 73, 105, 79, 111, 85, 117],
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
        }, {
          'type': 'Numbers',
          'count': 0
        }, {
          'type': 'Alphabet',
          'count': 0
        }
      ]
    }
  },
  methods: {
    formatCode: function () {
      this.countChars(this.inputText)
    },
    countChars: function (text) {
      this.counts[4].count = text.length
      let spaceCount = 0
      let vowCount = 0
      let conCount = 0
      let specialCharCount = 0
      let numCharCount = 0
      let alphaCount = 0
      for (let i = 0; i < text.length; i++) {
        let c = text.charCodeAt(i)
        if (c === 32) {
          spaceCount++
        } else if ((c >= 33 && c <= 47) ||
          (c >= 58 && c <= 64) ||
          (c >= 91 && c <= 96) ||
          (c >= 123 && c <= 126) ||
          (c >= 128 && c <= 140) ||
          (c === 142) ||
          (c >= 145 && c <= 156) ||
          (c >= 158)) {
          specialCharCount++
        } else if ((c >= 65 && c <= 90) || (c >= 97 && c <= 122)) {
          alphaCount++
          if (this.vowASCII.includes(c)) {
            vowCount++
          } else {
            conCount++
          }
        } else if (c >= 48 && c <= 57) {
          numCharCount++
        }
      }
      this.counts[0].count = vowCount
      this.counts[1].count = conCount
      this.counts[2].count = spaceCount
      this.counts[3].count = specialCharCount
      this.counts[5].count = numCharCount
      this.counts[6].count = alphaCount
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
