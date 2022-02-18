<template>
  <div id="app">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/app.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style lang="scss">

    body{
      padding: 0;
      margin: 0;
      box-sizing: border-box;
		background: rgb(35,63,70);
		background: linear-gradient(90deg, rgba(35, 63, 70, 0.61) 5%, rgba(104, 111, 117, 0.377) 50%, rgba(49, 67, 71, 0.63) 95%);
}
</style>
