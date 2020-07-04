<template>
  <div id="app">
    <Thubmnail />
  </div>
</template>

<script>
import Thubmnail from "./components/Thumbnail";

export default {
  name: "App",
  components: {
    Thubmnail
  }
};
</script>

<style>
html,
body,
#app {
  margin: 0;
  width: 100%;
  height: 100%;
}
</style>
