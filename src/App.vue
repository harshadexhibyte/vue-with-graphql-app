<template>
  <div id="app">
    <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
    <Singup />
    <img alt="Vue logo" src="./assets/logo.png" />
    <Home />
  </div>
</template>

<script>
// import HelloWorld from "./components/HelloWorld.vue";
import Home from "./components/home.vue";
import Singup from "./components/Singup.vue";
// import Singup from "./components/Singup.vue";
export default {
  name: "App",
  components: {
    // HelloWorld,
    Singup,
    Home,
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
