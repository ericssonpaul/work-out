<template>
  <div id="welcome">
    <h1><b>{{ welcome_msg }}</b></h1>
    <h2>{{ tagline_msg }}</h2>

    <div id="start-form-container">
      <div style="width: 100%;">
        <button class="event-button event-button-large" v-on:click="$emit('cc-update', 'sdadw')"><b>{{ button_start }}</b></button>
      </div>
      <div style="width: 100%;">
        <p style="margin: 20px;">{{ template_choose_msg }}</p>
      </div>
      <div class="preset-group-container">
        <div class="preset-group">
          <div v-for="preset in getRandomPresets(4)" :value="preset" class="preset-option">
            <button class="event-button event-button-small floated">{{ preset.name }}</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// We load the different presets from a json file
import presets from '../assets/data/presets.json'

export default {
  name: 'welcome',
  data () {
    return {
      welcome_msg: 'WORK-OUT — YOUR PERSONAL TRAINING GUIDE',
      tagline_msg: 'GET PERSONALIZED WARMUP AND STRETCHING EXERCISES',
      button_start: 'FIND MY PERFECT WARMUP',
      template_choose_msg: 'OR CHOOSE A PRESET',
      wo_config: {}
    }
  },
  methods: {
    // Take the list of presets and return 'num' random elements
    // *Note*: I know this is not the most optimal shuffling algoritm since it introduces quite a lot of bias.
    //         But for this specific task, it's not that important and it would take more time that it was worth to implement a better algoritm like Fisher-Yates.
    getRandomPresets: function (num) {
      return presets.presets.sort(function (a, b) { return 0.5 - Math.random() }).slice(0, num)
    }
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
.event-button {
  display: inline block;
  background-color: transparent;
  border: 2px solid black;
  border-color: #ffffff;
  border-radius: 20px;
  color: #ffffff;
  transition: 0.2s;
}
.event-button:hover {
  color: #353535;
  background-color: #ffffff;
  border-color: #ffffff;
}
.event-button:active {
  color: #353535;
  background-color: #e9e9e9;
  border-color: #e9e9e9;
}
.floated {
  float: left;
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
  .event-button-large {
    font-size: 20px;
    padding: 15px 30px 15px 30px;
  }
  .event-button-small {
    font-size: 20px;
    padding: 10px 20px 10px 20px;
    margin: 5px 20px 5px 25px;
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
  .event-button-large {
    font-size: 30px;
    padding: 25px 40px 25px 40px;
  }
  .event-button-small {
    font-size: 25px;
    padding: 15px 30px 15px 30px;
    margin: 10px;
  }
  .preset-group-container {
    margin-left: 2px;
    margin-right: 2px;
  }
}
</style>