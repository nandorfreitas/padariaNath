<template>
  <div id="app">
    <Padaria/>
  </div>
</template>

<script>
import Padaria from "./components/Padaria.vue";

export default {
  name: "App",
  components: {
    Padaria
  }
};
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
