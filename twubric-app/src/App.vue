<template>
<div id="app">
    <FollowerList />
  </div>
</template>

<script>
import FollowerList from './components/FollowerList.vue'

export default {
  name: 'App',
  components: {
    FollowerList
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
