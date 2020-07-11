<template>
  <div id="app">
    <Tracking />
  </div>
</template>

<script>
import Tracking from './components/Tracking.vue'

export default {
  name: 'App',
  components: {
    Tracking
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
