<template>
  <div class="landing">
    <div id="main-content">

      <v-container fluid fill-height>
        <v-layout align-center justify-center>
          <v-flex xs12 sm8 md10>
          <v-toolbar color="blue darken-3">
            <v-toolbar-items>
              <v-layout align-center="true" justify-center="true">
                <v-chip class="elevation-16 uk-animation-slide-left-small" color="secondary" small text-color="white" v-for="i in fitlerZeroCounts()" :key="i.type" v-on:click="highlight(i.type)">
                  <v-avatar class="teal">{{ i.count }}</v-avatar>
                  {{ i.type }}
                </v-chip>
              </v-layout>
            </v-toolbar-items>
          </v-toolbar>

          <v-snackbar
            :timeout="2000"
            :bottom="true"
            :top="false"
            :left="false"
            :right="false"
            v-model="snackbar"
          >
            {{ snackbarText }}
            <v-btn flat color="green" v-on:click="toggleSnackbar()">Close</v-btn>
          </v-snackbar>

          <v-card my-auto>
            <v-layout row>
              <v-flex md12 offset-xs0>
                <v-card class="card--flex-toolbar elevation-16" height="fit-content">
                  <v-toolbar color="darken-3" card prominent>
                    <v-text-field class="body-2 grey--text" id="chars-search" label="Search ..." v-model="search" row="1"></v-text-field>
                    <v-spacer></v-spacer>
                    <v-btn icon>
                      <v-icon color="blue darken1" @click="highlight('search', search)">search</v-icon>
                    </v-btn>
                    <v-btn icon v-on:click="deleteCode()" @click.native="snackbar = true">
                      <v-tooltip bottom>
                        <v-icon slot="activator" color="blue darken1">delete</v-icon>
                        <span>Clean text</span>
                      </v-tooltip>
                    </v-btn>
                  </v-toolbar>
                  <v-card-text style="height:max-content;">
                    <pre id= "chars-disp" v-if="viewMode" @click="enterEdit()" v-html="highlightedText">
                    </pre>
                    <v-text-field
                      id="chars-input"
                      name="input-1"
                      label="Paste or type your characters here"
                      multi-line
                      v-model="inputText"
                      @input="formatCode()"
                      rows="20"
                      :counter="10000000000"
                      v-else
                    ></v-text-field>
                  </v-card-text>
                </v-card>
              </v-flex>
            </v-layout>
          </v-card>
          </v-flex>
        </v-layout>
      </v-container>

    </div>

  </div>
</template>

<script>
/* eslint-disable */
export default {
  name: 'Landing',
  data () {
    return {
      imgUrl: 'static/BingWallpaper-2018-05-10.jpg',
      prevViewMode: "",
      viewMode: false,
      search: "",
      inputText: '',
			highlightedText: '',
      snacksTimeout: 2000,
      snackbarText: 'Nothing',
      snackbar: false,
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
          'type': 'Alphabets',
          'count': 0
        }, {
          'type': 'lower case',
          'count': 0
        }, {
          'type': 'ALL CAPS',
          'count': 0
        }
      ]
    }
  },
  methods: {
		enterView: function(){
      this.viewMode = true
    },
    enterEdit: function(){
      this.viewMode = false
      this.prevViewMode = ""
    },
    toggleSnackbar: function (snackbarText) {
      if (snackbarText) {
        this.snackbarText = snackbarText
        this.snackbar = !this.snackbar
      } else {
        this.snackbar = false
      }
    },
    fitlerZeroCounts: function () {
      let filteredCounts = []
      for (let i = 0; i < this.counts.length; i++) {
        if (this.counts[i].count > 0) {
          filteredCounts.push(this.counts[i])
        }
      }
      return filteredCounts
    },
    formatCode: function () {
      this.countChars(this.inputText)
    },
    deleteCode: function () {
      this.inputText = ''
      this.toggleSnackbar('Text cleared')
      this.countChars(this.inputText)
    },
    countChars: function (text) {
      this.counts[4].count = text.length
      let spaceCount = 0
      let vowCount = 0
      let conCount = 0
      let lowCapsCount = 0
      let allCapsCount = 0
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
          if (c >= 97 && c <= 122) {
            lowCapsCount++
          } else {
            allCapsCount++
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
      this.counts[7].count = lowCapsCount
      this.counts[8].count = allCapsCount
      },
      highlight: function(type, word){
      let regex = {
          "Vowels": /([aeiou])/gi,
          "Consonents": /([^aeiou])/gi,
          "Alphabets": /([a-z])/gi,
          "Numbers": /([\d]+)/gi,
          "lower case": /[a-z]/g,
          "ALL CAPS": /[A-Z]/g
      }
      if(regex.hasOwnProperty(type) && this.prevViewMode !== type){
          this.prevViewMode = type
          this.enterView()
          this.highlightedText = this.inputText.replace(regex[type], function(x){
            return "<span class= 'h'>"+x+"</span>"
          })
      } else if(type === "search" && this.prevViewMode !== word){
          this.prevViewMode = word
          this.enterView()
          this.highlightedText = this.inputText.replace(word, function(x){
            return "<span class= 'h'>"+x+"</span>"
          })
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
#chars-disp {
  display:block;
  width:100%;
  max-height:50vh;
  overflow-y:scroll;
  text-align:left;
  font-family: sans-serif;
  font-size:16px;
  font-weight:normal;
  color:white;
  border:0;
  outline:0;
  background-color: transparent;
}
</style>
<style>
/* style for the highlighted text */
#chars-disp .h {
  display:inline-block;
  color:yellow;
}
</style>
