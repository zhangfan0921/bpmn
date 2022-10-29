<template>
  <div id="app">
    <BmpnJs/>
  </div>
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import BmpnJs from './components/BmpnJs.vue'

export default {
  name: 'App',
  components: {
    //HelloWorld,
    BmpnJs
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
