<template>
  <div id="app">
    <compMain></compMain>
  </div>
</template>

<script>
import compMain from './components/Main';

export default {
  name: 'app',
  components: {
    "compMain": compMain
  }
}
</script>

<style lang="scss">

  html, body, #app {
    height: 100%;
    font-family: 'Roboto', sans-serif;
    background-color: #7f7f7f;
  }


</style>
