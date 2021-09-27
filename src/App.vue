<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <Paises/>
</template>

<script>
import Paises from './components/Paises.vue'
export default {
  name: 'App',
  components: {
    Paises
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
