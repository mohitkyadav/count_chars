<template>
<div>
    <v-container>
        <v-btn fab color="indigo">
            <v-tooltip bottom>
                <v-icon slot="activator">search</v-icon>
                <span>Search</span>
            </v-tooltip>
        </v-btn>
        <v-btn fab color="indigo" @click="deleteCode()" :disabled="disabled()" @click.native="snackbar = true">
            <v-tooltip bottom>
                <v-icon slot="activator">delete</v-icon>
                <span>Clear text</span>
            </v-tooltip>
        </v-btn>
        <v-btn fab color="indigo" @click="undo()" :disabled="undoStack <= 0">
            <v-tooltip bottom>
                <v-icon slot="activator">undo</v-icon>
                <span>Undo (ctrl+y)</span>
            </v-tooltip>
        </v-btn>
        <v-btn fab color="indigo" @click="redo()" :disabled="redoStack <= 0">
            <v-tooltip bottom>
                <v-icon slot="activator">redo</v-icon>
                <span>Redo (ctrl+z)</span>
            </v-tooltip>
        </v-btn>
    </v-container>

    <v-container fluid fill-height>
        <v-flex>
            <v-container>
                <v-chip class="elevation-16 uk-animation-slide-left-small ma-2" color="secondary" small text-color="white" v-for="i in fitlerZeroCounts()" :key="i.type">
                    <v-avatar class="indigo">{{ i.count }}</v-avatar>
                    {{ i.type }}
                </v-chip>
            </v-container>

            <v-snackbar :timeout="2000" :bottom="true" :top="false" :left="false" :right="false" v-model="snackbar">
                {{ snackbarText }}
                <v-btn flat color="white" v-on:click="toggleSnackbar()">Close</v-btn>
            </v-snackbar>

            <v-card>
                <v-card-text elevation=2>
                    <v-text-field id="chars-input" name="input-1" label="Paste your characters here" color="white" multi-line v-model="inputText" rows="20" :counter="10000000000" />
                </v-card-text>
            </v-card>
        </v-flex>
    </v-container>
</div>
</template>

<script>
export default {
  name: 'Landing',
  data () {
    return {
      undoStack: [],
      redoStack: [],
      inputText: '',
      snackbarText: 'Nothing',
      snackbar: false,

      /**
             * stackBuffer represents the number of chars
             * typed before storing inputText in the undoStack
             */
      stackBuffer: 5,
      vowASCII: [65, 97, 69, 101, 73, 105, 79, 111, 85, 117],
      counts: [{
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
      }]
    }
  },
  methods: {
    toggleSnackbar: function (snackbarText) {
      if (snackbarText) {
        this.snackbarText = snackbarText
        this.snackbar = !this.snackbar
      } else {
        this.snackbar = false
      }
    },
    disabled: function () {
      if (this.inputText === '') return true
      return false
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
    deleteCode: function () {
      this.inputText = ''
      this.toggleSnackbar('Text cleared')
    },
    undo: function () {
      this.redoStack.push(this.inputText)
      this.undoStack.pop()
      this.inputText = this.undoStack.pop()
    },
    redo: function () {
      if (this.redoStack.length > 0) { this.inputText = this.redoStack.pop() }
    },
    shouldPushToUndo: function () {
      if (this.largeAddition()) {
        this.undoStack.push(this.inputText)
      }
    },
    /**
         * largeAddition determins if changes too input
         * text qualify as a large enough addition to
         * store changes in the undoStack.
         *
         * There are three conditions, only one must be met:
         *
         * 1: Was inputText previously empty, if so,
         * store the first change in the undo stack
         *
         * 2 & 3: Is inputText larger/smaller by 'stackBuffer'
         * characters, this ensures undo stages are stored
         * as chunks of characters changes rather than single
         * characterchanges
         */
    largeAddition: function () {
      let previousInput = this.undoStack.pop()
      this.undoStack.push(previousInput)
      if (previousInput) {
        if (this.inputText.length > previousInput.length + 5 || this.inputText.length < previousInput.length - 5) { return true }
        return false
      }
      return true
    }
  },
  watch: {
    'inputText' () {
      if (!this.inputText) return
      this.shouldPushToUndo()
      this.counts[4].count = this.inputText.length
      let spaceCount = 0
      let vowCount = 0
      let conCount = 0
      let lowCapsCount = 0
      let allCapsCount = 0
      let specialCharCount = 0
      let numCharCount = 0
      let alphaCount = 0
      for (let i = 0; i < this.inputText.length; i++) {
        let c = this.inputText.charCodeAt(i)
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
    }
  }
}
</script>
