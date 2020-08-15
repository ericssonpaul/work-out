<template>
  <div id="qform">
    <transition appear name="fade" mode="out-in">
      <question :options="question" :key="question" @ret="onRet"></question>
    </transition>
  </div>
</template>

<script>
import question from './question'

import exercises from '../assets/data/exercises.json'

function shuffle (a) {
  for (let i = a.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [a[i], a[j]] = [a[j], a[i]]
  }
  return a
}

export default {
  name: 'qform',
  props: ['wo_config'],
  data () {
    return {
      configuration: undefined,
      question: undefined
    }
  },
  components: {
    question
  },
  // Generate the first question when the component loads
  created: function () {
    this.question = this.getQuestion()
  },
  methods: {
    // Returns a question to ask the user with
    // If all the necessary questions have been answered, then update contentComponent to display the result
    getQuestion: function () {
      if (this.configuration === undefined) {
        this.configuration = this.wo_config
      }

      // These are "special" questions, these are for example how much time the warmup should take etc
      if (this.configuration.people_low === undefined) {
        return {
          msg: 'How many are you?',
          id: 'people',
          type: 'choice',
          options: [
            'I\'m training alone',
            '2 - 5',
            '6 - 10',
            '11+'
          ]
        }
      }
      if (this.configuration.equipment === undefined) {
        return {
          msg: 'Do you have stretching equipment available? (elastic straps, stability balls, ...)',
          id: 'equipment',
          type: 'y/n',
          options: [
            'yes',
            'no'
          ]
        }
      }
      if (this.configuration.time_low === undefined) {
        return {
          msg: 'How much time do you have to warm up?',
          id: 'time',
          type: 'choice',
          options: [
            '1 - 3 minutes',
            '4 - 7 minutes',
            '8 - 10 minutes',
            '11 - 15 minutes'
          ]
        }
      }

      // Check which muscles haven't been checked
      const musclesChecked = this.configuration.muscles_checked
      const musclesEnable = this.configuration.muscles
      const musclesLeft = exercises.muscles.filter(function (el) {
        return !musclesChecked.includes(el) && !musclesEnable.includes(el)
      })

      // If all have been checked, then all necessary questions are answered; time to display result
      if (musclesLeft.length === 0) {
        this.$emit('conf-update', this.configuration)
        this.$emit('cc-update', 'result')
      }

      // Generate a multichoice question asking which muscles of four randomly generated options the user will use
      const muscles = shuffle(musclesLeft).slice(0, 4)
      return {
        msg: 'Which of these muscles are you going to use?',
        id: 'muscleselect',
        type: 'multichoice',
        options: muscles
      }
    },
    // Called by the question component once the answer to the question has been given
    onRet: function (data) {
      // Set the "special" questions
      if (this.question.id === 'people') {
        switch (data) {
          case 'I\'m training alone':
            this.configuration.people_low = 1
            this.configuration.people_high = 1
            break
          case '2 - 5':
            this.configuration.people_low = 2
            this.configuration.people_high = 5
            break
          case '6 - 10':
            this.configuration.people_low = 6
            this.configuration.people_high = 10
            break
          case '11+':
            this.configuration.people_low = 11
            this.configuration.people_high = 100
            break
        }
      }
      if (this.question.id === 'equipment') {
        if (data === 'yes') {
          this.configuration.equipment = true
        } else {
          this.configuration.equipment = false
        }
      }
      if (this.question.id === 'time') {
        switch (data) {
          case '1 - 3 minutes':
            this.configuration.time_low = 60
            this.configuration.time_high = 180
            break
          case '4 - 7 minutes':
            this.configuration.time_low = 240
            this.configuration.time_high = 420
            break
          case '8 - 10 minutes':
            this.configuration.time_low = 480
            this.configuration.time_high = 600
            break
          case '11 - 15 minutes':
            this.configuration.time_low = 660
            this.configuration.time_high = 900
            break
        }
      }
      if (this.question.id === 'muscleselect') {
        // Add toggled muscles to the muscles array in the config object
        this.configuration.muscles = this.configuration.muscles.concat(data)
        // And add all selectable options to the muscles_checked array
        this.configuration.muscles_checked = this.configuration.muscles_checked.concat(this.question.options)
      }

      // Finally, generate a new question
      this.question = this.getQuestion()
    }
  }
}
</script>

<style scoped>
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