<template>
  <div id="welcome">
    <h1><b>{{ welcome_msg }}</b></h1>
    <h2>{{ tagline_msg }}</h2>

    <div id="start-form-container">
      <div style="width: 100%;">
        <eventButton @click="$emit('cc-update', 'qform')" :message="button_start"></eventButton>
      </div>
      <div style="width: 100%;">
        <p style="margin: 20px;">{{ template_choose_msg }}</p>
      </div>
      <div class="preset-group-container">
        <div class="preset-group">
          <div v-for="preset in getRandomPresets(4)" :value="preset" class="preset-option">
            <optionButton :message="preset.name" @click="loadPreset(preset)"></optionButton>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import eventButton from './event-button'
import optionButton from './option-button'
// We load the different presets from a json file
import presets from '../assets/data/presets.json'
import exercises from '../assets/data/exercises.json'

export default {
  name: 'welcome',
  data () {
    return {
      welcome_msg: 'WORK-OUT — YOUR PERSONAL TRAINING GUIDE',
      tagline_msg: 'GET PERSONALIZED STRETCHING EXERCISES',
      button_start: 'FIND MY PERFECT WARMUP',
      template_choose_msg: 'OR CHOOSE A PRESET'
    }
  },
  methods: {
    // Take the list of presets and return 'num' random elements
    // *Note*: I know this is not the most optimal shuffling algoritm since it introduces quite a lot of bias.
    //         But for this specific task, it's not that important and it would take more time that it was worth to implement a better algoritm like Fisher-Yates.
    getRandomPresets: function (num) {
      return presets.presets.sort(function (a, b) { return 0.5 - Math.random() }).slice(0, num)
    },
    // Update config with the preset specified and change contentComponent to qform
    loadPreset: function (preset) {
      preset.config.muscles_checked = exercises.muscles
      this.$emit('conf-update', preset.config)
      this.$emit('cc-update', 'qform')
    }
  },
  components: {
    eventButton,
    optionButton
  }
}
</script>

<style scoped>
h1 {
  padding-top: 50px;
  font-weight: normal;
  display: inline-block;
}

#start-form-container {
  width: 100%;
}

.preset-option {
  display: inline-block;
  margin: 0 auto;
}

@media screen and (max-width: 1023px) {
  #start-form-container {
    padding-top: 25px;
  }
  h1 {
    padding-top: 50px;
    font-size: 30px;
  }
  h2 {
    font-size: 20px;
  }
  p {
    font-size: 15px;
  }
  .preset-group-container {
    margin-left: 5px;
    margin-right: 5px;
  }
}
@media screen and (min-width: 1024px) {
  #start-form-container {
    padding-top: 100px;
  }
  .preset-group-container {
    margin-left: 2px;
    margin-right: 2px;
  }
}
</style>