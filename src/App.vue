<template>
  <div id="app">
    <!--<img src="./assets/logo.png">-->
    <m-header></m-header>
    <tab></tab>
    <router-view></router-view>
  </div>
</template>

<script>
  import MHeader from './components/header/header'
  import Tab from '../src/components/tab/tab'
  export default {
    components: {
      MHeader,
      Tab
    }
  }
</script>

<style>
  /*@import "common/stylus/index.styl";*/
</style>
