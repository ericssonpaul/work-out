<template>
  <button v-on:click="$emit('click')"><b>{{ message }}</b></button>
</template>

<script>
export default {
  name: 'event-button',
  props: ['message']
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
}
button:hover {
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
    padding: 15px 30px 15px 30px;
  }
}
@media screen and (min-width: 1024px) {
  button {
    font-size: 30px;
    padding: 25px 40px 25px 40px;
  }
}
</style>