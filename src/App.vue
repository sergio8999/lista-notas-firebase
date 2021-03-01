<template>
  <div id="app">
    <cabecera />
    <contenido />
    <pie />
  </div>
  
</template>

<script>
import cabecera from "./components/cabecera.vue";
import contenido from "./components/contenido.vue";
import pie from "./components/pie.vue";

export default {
  name: 'App',
  components: {
    cabecera,
    contenido,
    pie
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
  background-color: black;
  height: 100vh;
}
</style>
