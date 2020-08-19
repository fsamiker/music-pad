<template>
  <div id="app">
    <MusicPad></MusicPad>
  </div>
</template>

<script>
import MusicPad from './components/MusicPad'

export default {
  name: 'App',
  components: {
    MusicPad
  }
}
</script>

<style>
#app {
  overflow: hidden;
  position: fixed;
  height: 100%;
  width: 100%;
  left: 0;
  top: 0;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: grid;
  place-items: center;
  text-align: center;
  color: #2c3e50;
  background-color: #312b2b;
}
</style>
