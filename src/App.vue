<template>
  <div id="app">
    <ImgurMain/>
  </div>
</template>

<script>
import ImgurMain from './components/ImgurMain'

export default {
  name: 'App',
  components: {
    ImgurMain
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #fff;
  height: 100%;
}

html, body {
  margin: 0;
  padding: 0;
  height: 100%;
  background-color: #141518;
}
h1, h2 {
  font-weight: normal;
}

a {
  color: #42b983;
}
</style>
