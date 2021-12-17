<template>
  <div class="weather-info">
    <div id="weather-today">
      <h2>Weather Today</h2>
      <div id="weather-today-right-now">
        <p>{{ todaysDate }}</p>
        <p>Ort: {{ weatherToday.name }}</p>
        <p>Land: {{ weatherToday.sys.country }}</p>
        <img
          :src="`https://openweathermap.org/img/wn/${this.weatherToday.weather[0].icon}@2x.png`"
          alt="icon"
        />
        <p>Väder: {{ weatherToday.weather[0].description }}</p>
        <p>Temperatur: {{ weatherToday.main.temp }}</p>
        <p>Känns som: {{ weatherToday.main.feels_like }}</p>
        <p>Lufttryck: {{ weatherToday.main.pressure }}</p>
        <p>Luftfuktighet: {{ weatherToday.main.humidity }}</p>
        <p>Vind hastighet: {{ weatherToday.wind.speed }}</p>
        <p>Vind deg: {{ weatherToday.wind.deg }}</p>
        <p>Vind gust: {{ weatherToday.wind.gust }}</p>
        <p>Sunrise: {{ weatherToday.sys.sunrise }}</p>
        <p>Sunset: {{ weatherToday.sys.sunset }}</p>
      </div>

      <div id="weather-today-forecast">
        <h2>Today Forecast</h2>
        <ul v-for="item in weatherTodayForecast" :key="item.dt_txt">
          <li>
            {{ item.dt_txt }}
          </li>
          <li>
            {{ item.main.temp }}
          </li>
          <li>
            {{ item.main.humidity }}
          </li>
          <li>
            {{ item.weather[0].description }}
          </li>
          <li>
            {{ item.wind.speed }}
          </li>
          <li>
            {{ item.wind.deg }}
          </li>
          <li>
            {{ item.wind.gust }}
          </li>
        </ul>
      </div>
    </div>

    <div id="weather-forecast">
      <h2>Weather Forecast</h2>
      <ul v-for="item in weatherForecastFiltered" :key="item.dt_txt">
        <li v-if="item.dt_txt.slice(11, 20) == '15:00:00'">
          <img
            :src="`https://openweathermap.org/img/wn/${item.weather[0].icon}@2x.png`"
            alt="icon"
          />
        </li>
        <li>
          {{ item.dt_txt }}
        </li>
        <li>
          {{ item.main.temp }}
        </li>
        <li>
          {{ item.main.humidity }}
        </li>
        <li>
          {{ item.weather[0].description }}
        </li>
        <li>
          {{ item.wind.speed }}
        </li>
        <li>
          {{ item.wind.deg }}
        </li>
        <li>
          {{ item.wind.gust }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Weather",
  data() {
    return {
      todaysDate: new Date().toLocaleString().substr(0, 10),
      lat: null,
      long: null,
      dayCounter: 0,
      weatherForecast: null,
      weatherForecastFiltered: [],
      weatherToday: null,
      weatherTodayForecast: [],
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
      await this.getWeatherForecast();
      // this.getWeatherTodayForecast();
    },
    getWeatherToday() {
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?lat=${this.lat}&lon=${this.long}&appid=${process.env.VUE_APP_WEATHER_API_KEY}`
        )
        .then((response) => (this.weatherToday = response.data));
    },
    getWeatherForecast: function () {
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/forecast?lat=${this.lat}&lon=${this.long}&appid=${process.env.VUE_APP_WEATHER_API_KEY}`
        )
        .then((response) => (this.weatherForecast = response.data.list))
        .then(() => {
          for (let i = 0; i < this.weatherForecast.length; i++) {
            if (
              this.weatherForecast[i].dt_txt.substr(0, 10) == this.todaysDate
            ) {
              this.weatherTodayForecast.push(this.weatherForecast[i]);
            } else {
              break;
            }
          }
        })
        .then(() => {
          for (let i = 0; i < this.weatherForecast.length; i++) {
            if (
              this.weatherForecast[i].dt_txt.substr(0, 10) !==
                this.todaysDate &&
              this.dayCounter < 3
            ) {
              if (
                this.weatherForecast[i].dt_txt.slice(11, 20) == "09:00:00" ||
                this.weatherForecast[i].dt_txt.slice(11, 20) == "15:00:00" ||
                this.weatherForecast[i].dt_txt.slice(11, 20) == "21:00:00"
              ) {
                this.weatherForecastFiltered.push(this.weatherForecast[i]);
                if (
                  this.weatherForecast[i].dt_txt.slice(11, 20) == "21:00:00"
                ) {
                  this.dayCounter++;
                }
              }
            }
          }
        });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
