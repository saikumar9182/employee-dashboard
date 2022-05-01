<template>
  <div id="app">
    <employe-content></employe-content>
  </div>
</template>

<script>
import EmployeContent from "./components/EmployeContent.vue";

export default {
  name: "App",
  components: {
    EmployeContent,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  /* background: #BED8FB; */
  background: #F9F9F9;
}
</style>
