<template>
  <div id="app">
    <stopwatch/>
  </div>
</template>

<script>
import stopwatch from './components/stopwatch'

export default {
  name: 'App',
  components: {
    stopwatch
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
