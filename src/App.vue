<template>
  <div id="app">
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <swiper1></swiper1>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import swiper from './components/swiper.vue'
import swiper1 from './components/swiper1.vue'

export default {
  name: 'app',
  components: {
    HelloWorld,
    swiper,
    swiper1
  }
}
</script>

<style>
#app {
  text-align: center;
}
</style>
