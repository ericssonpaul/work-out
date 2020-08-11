<template>
  <div class="app">
    <h1>{{ msg }}</h1>
    <h2>Get personalized exercises based on your needs!</h2>

    <button class="option-button">Start</button>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      msg: 'Work-Out - Your Personal Trainer'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
h1 {
  padding-top: 50px;
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #35495E;
}

.option-button {
  display: inline block;
  background-color: transparent;
  font-size: 30px;
  padding: 15px 15px 15px 15px;
  border: 2px solid black;
  border-color: #ffffff;
  border-radius: 15%;
  color: #ffffff;
  transition: 0.2s;
}
.option-button:hover {
  color: #353535;
  background-color: #ffffff;
  border-color: #ffffff;
}
.option-button:active {
  color: #353535;
  background-color: #e9e9e9;
  border-color: #e9e9e9;
}
</style>