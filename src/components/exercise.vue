<template>
  <div id="exercise">
    <h1>{{ exercise.name }}</h1>
    <p>Primary muscle stretched: {{ exercise.muscles[0] }}</p>
  </div>
</template>

<script>
export default {
  name: 'exercise',
  props: ['exercise']
}
</script>

<style scoped>
p {
  color: #7a7a7a;
}
@media screen and (max-width: 1023px) {
  h1 {
    font-size: 30px;
  }
}
</style>