<template>
  <div id="app">
    <header>
      <span><b><i>Work-Out</i></b></span>
    </header>
    <main>
      <!-- <img src="./assets/runner.svg" alt="Work-Out"> -->
      <router-view></router-view>
      <div class="footer">
        <ul>
          <li><p>© 2020 Jacob Paul</p></li>
          <li><a href="https://github.com/ericssonpaul/work-out" target="_blank" rel="noopener"><p>Code on GitHub</p></a></li>
        </ul>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app'
}
</script>

<style>
html {
  height: 100%;
}
body {
  margin: 0;
  height: calc(100% - 65px);
  padding: 0;
}

#app {
  height: 100%;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #ffffff;
}
a, p {
  color: #ffffff;
}

main {
  text-align: center;
  margin-top: 0px;
  height: 100%;
  background: rgb(24,209,0);
  background: linear-gradient(90deg, rgba(24,209,0,1) 0%, rgba(0,212,255,1) 100%);
}

header {
  margin: 0;
  height: 65px;
  background: rgb(24,209,0);
  background: linear-gradient(90deg, rgba(24,209,0,1) 0%, rgba(0,212,255,1) 100%);
  color: #ffffff;
}

header span {
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

.footer {
  position: fixed;
  bottom: 0;
  width: 100%;
}
</style>
