<template>
  <div id="result">
    <div v-if="exercises === undefined" id="find-error">
      <h1><b>{{ find_error_msg }}</b></h1>
      <h2>{{ find_error_exp }}</h2>
    </div>
    <div v-if="exercises != undefined" id="exercises-container-margin">
      <div id="exercises-container">
        <div id="navigation">
          <a v-on:click="onBackArrowClick()"    v-if="exercise_current !== 0"                    style="float: left;" ><b>⯇</b></a>
          <a v-on:click="onForwardArrowClick()" v-if="exercise_current !== exercises.length - 1" style="float: right;"><b>⯈</b></a>
        </div>
        <transition appear name="fade" mode="out-in">
          <exerciseComponent :exercise="exercises[exercise_current]" :key="exercise_current"/>
        </transition>
      </div>
    </div>
    <div id="retry-button-container">
      <eventButton @click="onRetryClick()" :message="button_retry"/>
    </div>
  </div>
</template>

<script>
import exerciseComponent from './exercise'
import eventButton from './event-button'

import exercises from '../assets/data/exercises.json'

export default {
  name: 'result',
  props: ['wo_config'],
  data () {
    return {
      find_error_msg: 'Could not find any suitable exercises nor stretches!',
      find_error_exp: 'Please try again',
      button_retry: 'RETRY',
      configuration: undefined,
      exercises: undefined,
      exercise_current: 0
    }
  },
  // When the result component is loaded, we try to find the best warmup layout based on the setting that get passed by the parent as wo_config
  created: function () {
    var configuration = this.wo_config
    var stretches = exercises.stretches
    // First, we score all the differtent stretches based on what muscles are included
    for (let i = 0; i < stretches.length; i++) {
      stretches[i].score = 0
      for (let t = 0; t < configuration.muscles.length; t++) {
        if (stretches[i].muscles.includes(configuration.muscles[t])) {
          stretches[i].score += configuration.muscles.length - t
        }
      }
    }
    // Remove all stretches with a score of 0 or that equipment is required (and the user has explicitly said no to equipment)
    stretches = stretches.filter(function (el) {
      return el.score !== 0 && ((el.equipment && configuration.equipment) || !el.equipment)
    })
    if (stretches.length === 0) {
      this.exercises = undefined
      return
    }
    // Sort in descending order
    stretches.sort(function (a, b) {
      return a.score - b.score
    }).reverse()
    // Calculate the number of stretches we fit in our timeframe
    let time = 0
    let i
    for (i = 0; i < stretches.length; i++) {
      if (time + stretches[i].time > configuration.time_high) {
        break
      }
      time += stretches[i].time
    }
    // if (time < configuration.time_low) {
    //   this.exercises = undefined
    //   return
    // }
    stretches = stretches.slice(0, i + 1)

    this.exercises = stretches
  },
  methods: {
    // Called if the user requests to reset the page by clicking the retry button
    onRetryClick: function () {
      location.reload()
    },
    onBackArrowClick: function () {
      if (this.exercise_current !== 0) {
        this.exercise_current--
      }
    },
    onForwardArrowClick: function () {
      if (this.exercise_current !== this.exercises.length - 1) {
        this.exercise_current++
      }
    }
  },
  components: {
    exerciseComponent,
    eventButton
  }
}
</script>

<style scoped>
#find-error h1 {
  padding-top: 50px;
  font-weight: normal;
  display: inline-block;
}
#find-error h2 {
  margin-bottom: 100px;
}
#exercises-container-margin {
  padding-top: 80px;
  padding-bottom: 20px;
  max-width: 1024px;
  height: 70vh;
  margin-left: auto;
  margin-right: auto;
}
#exercises-container {
  height: 100%;
  display: inline block;
  background-color: #ffffff;
  border-radius: 20px;
  color: #000000;
}
#navigation {
  margin: 0;
  height: 65px;
  border-style: none none solid none;
  border-bottom-color: #000000;
  border-width: thin; 
}
a {
  text-decoration: none;
  cursor: pointer;
  display: block;
  position: relative;
  font-size: 40px;
  box-sizing: border-box;
  padding-left: 20px;
  padding-right: 20px;
}
@media screen and (max-width: 1023px) {
  h1 {
    font-size: 30px;
  }
  #find-error h1 {
    padding-top: 50px;
  }
  h2 {
    font-size: 20px;
  }
  #exercises-container-margin {
    padding-top: 20px;
  }
}
@media screen and (min-width: 1024px) {
  
}

.fade-enter-active {
  transition: opacity 0.5s ease-in-out;
}
.fade-enter-to {
  opacity: 1;
}
.fade-enter {
  opacity: 0;
}
.fade-leave-active {
  transition: opacity 0.3s ease-in-out;
}
.fade-leave-to {
  opacity: 0;
}
.fade-leave {
  opacity: 1;
}
</style>