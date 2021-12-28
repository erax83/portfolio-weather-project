<template>
  <div class="weather-info">
    <div id="weather-today">
      <h2>Todays Weather, {{ todaysDate }}</h2>
      <div id="weather-today-right-now">
        <img
          id="img-now"
          :src="`https://openweathermap.org/img/wn/${this.weatherToday.weather[0].icon}@2x.png`"
          alt="weathericon"
        />
        <p>{{ lat }}, {{ long }}</p>
        <p>{{ weatherToday.weather[0].description }}</p>
        <h1>Temperature: {{ weatherToday.main.temp }} °</h1>
        <p>Känns som: {{ weatherToday.main.feels_like }} °</p>
        <p>Lufttryck: {{ weatherToday.main.pressure }}</p>
        <p>Luftfuktighet: {{ weatherToday.main.humidity }}</p>
        <p>Vind hastighet: {{ weatherToday.wind.speed }}</p>
        <p>Vind deg: {{ weatherToday.wind.deg }}</p>
        <p>Sunrise: {{ weatherToday.sys.sunrise }}</p>
        <p>Sunset: {{ weatherToday.sys.sunset }}</p>
      </div>

      <div id="weather-today-forecast">
        <h2>Today Forecast</h2>
        <ul v-for="item in weatherTodayForecast.slice(0, 6)" :key="item.dt_txt">
          <li>
            {{ item.dt_txt.toLocaleString().substr(11, 5) }}
          </li>
          <li>{{ item.main.temp }} °</li>
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
          <li>
            <img
              :src="`https://openweathermap.org/img/wn/${item.weather[0].icon}@2x.png`"
              alt="icon"
            />
          </li>
        </ul>
      </div>
    </div>

    <div id="weather-forecast">
      <h2>Weather Forecast</h2>
      <div
        v-for="item in weatherForecastFiltered"
        :key="item.dt_txt"
        id="forecast-list"
      >
        <ul
          v-if="item.dt_txt.slice(11, 20) == '15:00:00'"
          class="forecast-wrapper"
        >
          <div class="box-one">
            <li>
              <img
                :src="`https://openweathermap.org/img/wn/${item.weather[0].icon}@2x.png`"
                alt="icon"
              />
            </li>
            <li>
              {{ item.weather[0].description }}
            </li>
            <li>
              {{ item.dt_txt.slice(0, 10) }}
            </li>
            <li>{{ item.main.temp }} °</li>
          </div>
          <div class="box-two">
            <li>
              {{ item.main.humidity }}
            </li>
            <li>
              {{ item.wind.speed }}
            </li>
            <li>
              {{ item.wind.deg }}
            </li>
          </div>
        </ul>
      </div>
    </div>

    <div id="weather-forecast">
      <h2>Weather Forecast</h2>
      <table
        v-for="item in weatherForecastFiltered"
        :key="item.dt_txt"
        id="forecast-list"
      >
        <tbody
          v-if="item.dt_txt.slice(11, 20) == '15:00:00'"
          class="forecast-wrapper"
        >
          <tr class="box-one">
            <td>
              <img class="icon-img"
                :src="`https://openweathermap.org/img/wn/${item.weather[0].icon}@2x.png`"
                alt="icon"
              />
            </td>
            <td>
              {{ item.weather[0].description }}
            </td>
            <td>
              {{ item.dt_txt.slice(0, 10) }}
            </td>
            <td>{{ item.main.temp }} °</td>
            <td>
              {{ item.main.humidity }}
            </td>
            <td>
              {{ item.wind.speed }}
            </td>
            <td>
              {{ item.wind.deg }}
            </td>
          </tr>
          <tr>
            <td>
              {{ item.main.humidity }}
            </td>
            <td>
              {{ item.wind.speed }}
            </td>
            <td>
              {{ item.wind.deg }}
            </td>
          </tr>
        </tbody>
      </table>
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
      tomorrowsDate: new Date(new Date().setDate(new Date().getDate() + 1))
        .toLocaleString()
        .substr(0, 10),
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
          `https://api.openweathermap.org/data/2.5/weather?lat=${this.lat}&lon=${this.long}&appid=${process.env.VUE_APP_WEATHER_API_KEY}&units=metric`
        )
        .then((response) => (this.weatherToday = response.data));
    },
    getWeatherForecast: function () {
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/forecast?lat=${this.lat}&lon=${this.long}&appid=${process.env.VUE_APP_WEATHER_API_KEY}&units=metric`
        )
        .then((response) => (this.weatherForecast = response.data.list))
        .then(() => {
          for (let i = 0; i < this.weatherForecast.length; i++) {
            if (
              this.weatherForecast[i].dt_txt.substr(0, 10) == this.todaysDate ||
              this.weatherForecast[i].dt_txt.substr(0, 10) == this.tomorrowsDate
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
<style scoped>
/* #img-now {
  width: 150px;
  padding: 0;
} */
#weather-forecast {
  background: lightcyan;
}

.icon-img {
  width: 2.5em;
}

/* .forecast-wrapper {
  display: flex;
  flex-flow: column wrap;
  height: 375px;
  justify-content: space-between;
} */
/* 
.box-one {
  grid-row-start: 0;
  grid-row-end: 1;
  grid-column-start: 0;
  grid-column-end: 3;
  justify-self: stretch;
}

.box-two {
  grid-row-start: 1;
  grid-row-end: 2;
  grid-column-start: 0;
  grid-column-end: 3;
  display: flex;
} */
</style>
