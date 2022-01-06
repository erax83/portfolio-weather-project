<template>
  <div id="app" class="grid-container">
    <!-- <Header class="header"/> -->
    <Weather class="weather" />
  </div>
</template>

<script>
import Weather from "./components/Weather.vue";
// import Header from "./components/Header.vue";

export default {
  name: "App",
  components: {
    Weather,
    // Header,
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Sorts+Mill+Goudy&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Abhaya+Libre&family=Sorts+Mill+Goudy&display=swap");

#app {
  /* font-family: Avenir, Helvetica, Arial, sans-serif; */
  font-family: "Sorts Mill Goudy", serif;
  font-family: "Abhaya Libre", serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* color: #1c1c1e; */
  line-height: 1em;
  margin: auto;
  max-width: 1000px;
  padding: 5px;
}

/* .grid-container {
  display: grid;
  grid-template-rows: 500px 1fr;
}

.header {
  grid-row-start: 0;
  grid-row-end: 1;
  margin: 0px;
}

.weather {
  grid-row-start: 1;
  grid-row-end: 2;
} */

h1 {
  font-family: "Sorts Mill Goudy", serif;
  font-size: 1.3em;
}

h2 {
  /* font-family: 'Sorts Mill Goudy', serif; */
  font-size: 1.2em;
}

ul {
  list-style-type: none;
}
</style>
