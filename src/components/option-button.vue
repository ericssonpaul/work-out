<template>
  <button v-on:click="onClick()" :class="{ active: active }">{{ message }}</button>
</template>

<script>
export default {
  name: 'option-button',
  props: ['message', 'selectable'],
  data () {
    return {
      active: false
    }
  },
  methods: {
    onClick: function () {
      if (this.selectable === true) {
        this.active = !this.active
      }
      this.$emit('click')
    }
  }
}
</script>

<style scoped>
button {
  display: inline block;
  background-color: transparent;
  border: 2px solid black;
  border-color: #ffffff;
  border-radius: 20px;
  color: #ffffff;
  transition: 0.2s;
  float: left;
}
button:hover, .active {
  color: #353535;
  background-color: #ffffff;
  border-color: #ffffff;
}
button:active {
  color: #353535;
  background-color: #e9e9e9;
  border-color: #e9e9e9;
}
@media screen and (max-width: 1023px) {
  button {
    font-size: 20px;
    padding: 10px 20px 10px 20px;
    margin: 5px 8px;
  }
}
@media screen and (min-width: 1024px) {
  button {
    font-size: 25px;
    padding: 15px 30px 15px 30px;
    margin: 10px;
  }
}
</style>