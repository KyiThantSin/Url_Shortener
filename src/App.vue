<template>
  <h1>Url Shortener ^^</h1>
  <Shortener />
</template>

<script>
import Dellater from './components/Shortener.vue'
import Shortener from './components/Shortener.vue'

export default {
  name: 'App',
  components: {
    Shortener
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
