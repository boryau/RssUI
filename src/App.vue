<template>
  <div id="app">
    <RssCrawlerDisplay/>
  </div>
</template>

<script>
import RssCrawlerDisplay from './components/RssCrawlerDisplay'

export default {
  name: 'App',
  components: {
    RssCrawlerDisplay
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
