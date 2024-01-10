<template>
  <AllTypesWidget />
</template>

<script>
import "bootstrap-icons/font/bootstrap-icons.css";
import "bootstrap/dist/css/bootstrap.min.css";
import "./assets/style.css";
import AllTypesWidget from "./components/AllTypesWidget.vue";

export default {
  name: "App",
  components: {
    AllTypesWidget,
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
