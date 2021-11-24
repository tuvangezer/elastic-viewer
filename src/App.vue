<template>
  <SearchPage msg="Welcome to Your Vue.js App"/>
</template>

<script>
import SearchPage from './components/SearchPage.vue'

export default {
  name: 'App',
  components: {
    SearchPage
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
  margin-top: 20px;
}
body {
  background-color: rgb(250,250,250);
}
</style>
