<template>
  <div id="navbar">
    <header>
      <a href="/"><b><i>{{ title }}</i></b></a>
    </header>
  </div>
</template>

<script>
export default {
  name: 'navbar',
  data () {
    return {
      title: 'Work-Out'
    }
  }
}
</script>

<style scoped>
header {
  margin: 0;
  height: 65px;
  background: rgb(24,209,0);
  background: linear-gradient(90deg, rgba(24,209,0,1) 0%, rgba(0,212,255,1) 100%);
  color: #ffffff;
}
header a {
  color: #ffffff;
  text-decoration: none;
  display: block;
  position: relative;
  font-size: 25px;
  line-height: 1;
  letter-spacing: .02em;
  font-weight: 400;
  box-sizing: border-box;
  padding-top: 25px;
  padding-bottom: 15px;
  padding-left: 20px;
  border-style: none none solid none;
  border-bottom-color: #ffffff;
  border-width: thin; 
}
</style>