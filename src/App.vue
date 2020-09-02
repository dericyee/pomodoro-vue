<template>
  <div id="app">
    <Pomodoro />
  </div>
</template>

<script>
import Pomodoro from "./components/Pomodoro.vue";

export default {
  name: "App",
  components: {
    Pomodoro,
  },
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  background: #42b883;
}
</style>
