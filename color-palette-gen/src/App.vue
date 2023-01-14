<template>
  <div class="wrapper">
  <ColorPalette/>
  </div>
</template>

<script>
import ColorPalette from './components/Color-Palette.vue'

export default {
  name: 'App',
  components: {
    ColorPalette
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
html, body{
 
  background-color: #A9C9FF;

}
.wrapper{


}

</style>
