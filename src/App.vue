<template>
  <div id="app">
    <!-- Title navbar -->
    <navbar></navbar>
    <main>
      <!-- The main content, defined by 'contentComponent' and changed as needed -->
      <transition appear name="fade">
        <component v-bind:is="contentComponent" @cc-update="updateContentComponent"></component>
      </transition>
      <!-- Footer (CC 2020, GitHub etc) -->
      <footersm></footersm>
    </main>
  </div>
</template>

<script>
// Here are the primary content components (those that 'contentComponent' can be set to)
import welcome from './components/welcome'

// Generic components
import navbar from './components/navbar'
import footersm from './components/footersm'

export default {
  name: 'app',
  data () {
    return {
      contentComponent: 'welcome' // The welcome screen is our initial component
    }
  },
  components: {
    welcome,
    navbar,
    footersm
  },
  methods: {
    // Invoked by the current content component to change the primary content.
    // For example, the welcome component calls this function when the user presses on one of the buttons
    updateContentComponent: function (value) {
      this.contentComponent = value
    }
  }
}
</script>

<style>
html {
  height: 100%;
}
body {
  margin: 0;
  height: 100%;
  padding: 0;
}

#app {
  height: 100%;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #ffffff;
}

main {
  text-align: center;
  margin-top: 0px;
  height: calc(100% - 65px);
  background: rgb(24,209,0);
  background: linear-gradient(90deg, rgba(24,209,0,1) 0%, rgba(0,212,255,1) 100%);
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
