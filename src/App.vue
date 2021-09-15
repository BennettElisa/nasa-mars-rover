<template>
  <NavBar />
  <router-view />
  <Footer />
</template>

<script>
import NavBar from "./components/NavBar.vue";
import Footer from "./components/Footer.vue";
import Home from "./views/Home.vue";

export default {
  name: "App",
  components: {
    Home,
    Footer,
    NavBar
  }
};
</script>

<style>
#app {
  font-family: "Montserrat";
  text-align: center;
  color: white;
  height: 100vh;
  width: 100vw;
}
</style>
