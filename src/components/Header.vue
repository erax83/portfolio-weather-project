<template>
  <div id="header">
    <h1>Local Weather Report</h1>
    <div>
      <h2><img src="@/assets/map.png" alt="map-pointer" class="map-pointer" /> {{ city }}, {{ country }}</h2>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Header",
  data() {
    return {
      city: "",
      country: "",
    };
  },
  mounted() {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(this.getPositionByCordinates);
    }
  },
  methods: {
    getPositionByCordinates: async function (position) {
      this.lat = await position.coords.latitude;
      this.long = await position.coords.longitude;
      await this.getWeatherToday();
    },
    getWeatherToday() {
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?lat=${this.lat}&lon=${this.long}&appid=${process.env.VUE_APP_WEATHER_API_KEY}`
        )
        .then((response) => {
          this.city = response.data.name;
          this.country = response.data.sys.country;
        });
    },
  },
};
</script>

<style scoped>
.map-pointer {
  width: 18px;
}
</style>
