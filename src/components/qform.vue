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
  created: function () {
    this.question = this.getQuestion()
  },
  methods: {
    getQuestion: function () {
      if (this.configuration === undefined) {
        this.configuration = this.wo_config
      }

      const musclesChecked = this.configuration.muscles_checked
      const musclesEnable = this.configuration.muscles
      const musclesLeft = exercises.muscles.filter(function (el) {
        return !musclesChecked.includes(el) && !musclesEnable.includes(el)
      })

      if (musclesLeft.length === 0) {
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
        this.$emit('conf-update', this.configuration)
        this.$emit('cc-update', 'result')
      }

      const muscles = shuffle(musclesLeft).slice(0, 4)
      return {
        msg: 'Which of these muscles are you going to use?',
        id: 'muscleselect',
        type: 'multichoice',
        options: muscles
      }
    },
    onRet: function (data) {
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
        this.configuration.muscles = this.configuration.muscles.concat(data)
        this.configuration.muscles_checked = this.configuration.muscles_checked.concat(this.question.options)
      }

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