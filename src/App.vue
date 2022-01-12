<template>
  <div id="app">
    <Weather class="weather" />
  </div>
</template>

<script>
import Weather from "./components/Weather.vue";

export default {
  name: "App",
  components: {
    Weather,
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Sorts+Mill+Goudy&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Abhaya+Libre&family=Sorts+Mill+Goudy&display=swap");

html {
  scroll-behavior: smooth;
}

#app {
  font-family: "Abhaya Libre", serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  line-height: 1em;
  margin: auto;
  max-width: 1000px;
  padding: 5px;
}

h1,
h2,
h3 {
  font-family: "Sorts Mill Goudy", serif;
  font-size: 1.3em;
}

h2 {
  font-size: 1.1em;
}

h3 {
  font-size: 1.1em;
}

/* .hr-class {
  margin-top: 30px;
} */
</style>
