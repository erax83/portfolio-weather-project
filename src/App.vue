<template>
  <div id="app" class="grid-container" >
    <Header class="header"/>
    <Weather class="weather"/>
  </div>
</template>

<script>
import Weather from "./components/Weather.vue";
import Header from "./components/Header.vue";

export default {
  name: "App",
  components: {
    Weather,
    Header,
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
  /* color: #1c1c1e; */
  line-height: 1.4;
  margin: 0px;
}

.grid-container {
  grid-template-rows: 100px 1fr;
}

.header {
  grid-row-start: 0;
  grid-row-end: 1;
  background: yellow;
  margin: 0px;
}

.weather {
  grid-row-start: 1;
  grid-row-end: 2;
  background: lightblue;
}

ul {
  list-style-type: none;
}
</style>
