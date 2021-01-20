<template>
  <div id="app">
    <calculadora />
  </div>
</template>

<script>
import Calculadora from './components/Calculadora.vue'

export default {
  name: 'App',
  components: {
    Calculadora
  }
}
</script>

<style lang="scss">
#app {
height: 100vh;
margin: auto;
}
</style>
