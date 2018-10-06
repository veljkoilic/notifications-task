<template>
  <div id="app">
    <NotificationBox msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import NotificationBox from './components/NotificationBox.vue'

export default {
  name: 'app',
  components: {
    NotificationBox
  }
}
</script>

<style lang="scss">
  body{
    margin: 0;
    padding: 0;
    background: #EFF3F6;
    overflow: hidden;
  }
#app {
  font-family: 'Raleway', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

}
</style>
