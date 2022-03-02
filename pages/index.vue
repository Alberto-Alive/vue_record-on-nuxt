<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <div>
        <v-card class="mb-4">
          <v-card-text :v-model="e1">
            Question {{ e1 }}: {{ questions[`${e1-1}`].text }}
          </v-card-text>
        </v-card>
        <v-stepper v-model="e1">
          <v-stepper-header>
            <template v-for="n in steps">
              <v-stepper-step
                :key="`${n}-step`"
                :complete="e1 > n"
                :step="n"
                editable
              >
                Step {{ n }}
              </v-stepper-step>

              <v-divider v-if="n !== steps" :key="n"></v-divider>
            </template>
          </v-stepper-header>

          <v-stepper-items>
            <v-stepper-content
              v-for="n in steps"
              :key="`${n}-content`"
              :step="n"
            >
              <v-card
                class="mb-12"
                color="grey lighten-1"
                height="200px"
              ><div class="item">
      <span>Videos</span>
      <VueRecordVideo @result="onResultVideo" mode="press" />
      <video
        v-for="(video, index) in videos"
        :src="video"
        :key="index"
        controls="controls"
      />
    </div></v-card>

              <v-btn color="primary" @click="nextStep(n)"> Continue </v-btn>

              <v-btn text> Cancel </v-btn>
            </v-stepper-content>
          </v-stepper-items>
        </v-stepper>
      </div>
    </v-col>
  </v-row>
</template>
<script>
export default {
  name: "Test",
  props: {},
  data() {
    return {
      e1: 1,
      steps: 5,
      audios: [],
      videos: [],
      questions:[
        {text: 'What is q1?'},
        {text: 'What is q2?'},
        {text: 'What is q3?'},
        {text: 'What is q4?'},
        {text: 'What is q5?'},
      ]
    }
  },

  watch: {
    steps(val) {
      if (this.e1 > val) {
        this.e1 = val
      }
    },
  },

  methods: {
    nextStep(n) {
      if (n === this.steps) {
        this.e1 = 1
      } else {
        this.e1 = n + 1
      }
    },
    onResultVideo(blob) {
      this.videos.push(window.URL.createObjectURL(blob));
    },
  }
}
</script>
