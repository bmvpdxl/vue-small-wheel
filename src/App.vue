<template>
  <div id="app">

    <custom-input></custom-input>

  </div>
</template>

<script>
  import CustomInput from './components/CustomInput'

  export default {
    name: 'app',
    data() {
      return {
        msg: 'Welcome to Your Vue.js App'
      }
    },
    components: {
      CustomInput,
    },
  }
</script>

<style lang="scss">
  body {
    margin: 0;
  }

  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    margin-top: 60px;
  }
</style>
