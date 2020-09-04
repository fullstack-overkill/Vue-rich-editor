<template>
  <div id="app">
    <editor> </editor>
  </div>
</template>

<script>
import editor from "./components/editor/editor.vue";

export default {
  name: "App",
  components: {
    editor,
  },
};
</script>

<style lang="stylus">
#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  min-height: 100vh;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: #F6F2EF;
  color: #2c3e50;
}
</style>
