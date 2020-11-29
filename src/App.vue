<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/mtg.jpg" class="mtg-logo">
    <HelloWorld msg="Roll for a commander"/>
  </div>
</template>

<script>
import HelloWorld from './components/Form.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
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

.mtg-logo {
  width: 200px;
}
</style>
