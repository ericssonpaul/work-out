<template>
  <div id="footersm">
      <ul>
        <li><p>© {{ year }} {{ name }}</p></li>
        <li><a :href="github_link" target="_blank" rel="noopener"><p>{{ github_caption }}</p></a></li>
      </ul>
  </div>
</template>

<script>
export default {
  name: 'footersm',
  data () {
    return {
      year: '2020',
      name: 'Jacob Paul',
      github_link: 'https://github.com/ericssonpaul/work-out',
      github_caption: 'Code on GitHub'
    }
  }
}
</script>

<style scoped>
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a, p {
  color: #ffffff;
}
#footersm {
  position: fixed;
  bottom: 0;
  width: 100%;
}
</style>