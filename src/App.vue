<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <H5player />
</template>

<script>
import H5player from "./components/H5Player.vue";
export default {
  name: "App",
  components: {
    H5player,
  },
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
