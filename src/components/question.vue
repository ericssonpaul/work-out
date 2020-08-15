<template>
  <div id="question">
    <h1><b>{{ options.msg }}</b></h1>
    <div id="question-type-message">
      <div v-if="options.type === 'y/n'">
        <h2>{{ yesno_description }}</h2>
      </div>
      <div v-if="options.type === 'multichoice'">
        <h2>{{ multichoice_description }}</h2>
      </div>
      <div v-if="options.type === 'choice'">
        <h2>{{ choice_description }}</h2>
      </div>
    </div>
    <div id="question-options-container">
      <div v-for="option in options.options" :value="option" class="question-option">
        <optionButton :message="option.charAt(0).toUpperCase() + option.slice(1)" :selectable="true" @click="onOptionClick(option)"/>
      </div>
    </div>
    <div v-if="options.type === 'multichoice'" id="multichoice-next-container">
      <eventButton :message="multichoice_next_msg" @click="onMultichoiceNextClick"/>
    </div>
  </div>
</template>

<script>
import optionButton from './option-button'
import eventButton from './event-button'

export default {
  name: 'question',
  props: ['options'],
  data () {
    return {
      multichoice_next_msg: 'Next →',
      yesno_description: 'Select either YES or NO',
      multichoice_description: 'Select any number of options, then click NEXT',
      choice_description: 'Select one',
      multichoice_pressed: []
    }
  },
  components: {
    optionButton,
    eventButton
  },
  methods: {
    // Called when the user click on any option button
    onOptionClick: function (option) {
      if (this.options.type === 'choice' || this.options.type === 'y/n') {
        // If question type is y/n or single choice, then we're done here: just emit back to the parent what the user clicked
        this.$emit('ret', option)
      } else {
        // If the question type is multichoice, then it's a different story.
        // We either append or remove the option the user toggled to the multichoice_pressed array
        if (!this.multichoice_pressed.includes(option)) {
          this.multichoice_pressed.push(option)
        } else {
          this.multichoice_pressed.splice(this.multichoice_pressed.indexOf(option), 1)
        }
      }
    },
    // Called when the user clicks on the "Next" button only present if the question type is multichoice
    onMultichoiceNextClick: function () {
      // Just emit back to the parent which options were toggled
      this.$emit('ret', this.multichoice_pressed)
      this.multichoice_pressed = []
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
#question-options-container {
  width: 100%;
}
#multichoice-next-container {
  width: 100%;
  padding-top: 20px;
}
.question-option {
  display: inline-block;
  margin: 0 auto;
}
@media screen and (max-width: 1023px) {
  h1 {
    padding-top: 50px;
    font-size: 30px;
  }
  h2 {
    font-size: 15px;
    padding-bottom: 15px;
  }
}
@media screen and (min-width: 1024px) {
  h2 {
    font-size: 20px;
    padding-bottom: 30px;
  }
}
</style>