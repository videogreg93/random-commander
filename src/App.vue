<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/mtg.png" class="mtg-logo">
    <Form msg="Roll for a commander"/>
    <Footer/>
  </div>
</template>

<script>
import Form from './components/Form.vue'
import Footer from './components/Footer.vue'

export default {
  name: 'App',
  components: {
    Form, Footer
  }
}
</script>

<style>
body {
  background: url("assets/background.png");
}
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
