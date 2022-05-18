<template>
  <div id="app">
    <img width="25%" src="./assets/logo.png" />
    　<EventTimeManagement msg="Hello Composition API in CodeSandbox!" />
  </div>
</template>

<script>
import EventTimeManagement from "./components/EventTimeManagement.vue";
export default {
  name: "App",
  components: {
    EventTimeManagement,
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
