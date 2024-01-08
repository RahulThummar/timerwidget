<template>
  <AllTypesWidget />
  <!-- <TimerWidget />
  <TrafficLightWidget /> -->
</template>

<script>
import "bootstrap-icons/font/bootstrap-icons.css";
import "bootstrap/dist/css/bootstrap.min.css";
import "./assets/style.css";
// import TimerWidget from "./components/TimerWidget.vue";
// import TrafficLightWidget from "./components/TrafficLightWidget.vue";
import AllTypesWidget from "./components/AllTypesWidget.vue";

export default {
  name: "App",
  components: {
    // TimerWidget,
    // TrafficLightWidget,
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
