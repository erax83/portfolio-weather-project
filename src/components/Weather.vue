<template>
  <div class="weather-info">
    <header>
      <div class="title">
        <h1>Local Weather Report</h1>
      </div>
      <br />
      <div class="place">
        <p>
          <img src="@/assets/map.png" alt="map-pointer" class="map-pointer" />
          {{ weatherToday.name }}, {{ weatherToday.sys.country }}
        </p>
      </div>
    </header>
    <hr />
    <div id="weather-today">
      <div class="test">
        <h2 class="today-headline">Todays Weather, {{ todaysDate }}</h2>
        <p class="x"><a href="#weather-forecast">Forecast ↓</a></p>
      </div>

      <div id="weather-today-right-now">
        <img
          class="big-weather-icon weather-icon"
          id="img-now"
          :src="`https://openweathermap.org/img/wn/${this.weatherToday.weather[0].icon}@2x.png`"
          alt="weathericon"
        />
        <!-- <p>{{ lat }}, {{ long }}</p> -->
        <p>{{ weatherToday.weather[0].description }}</p>
        <h3>
          <img class="data-img" src="@/assets/temp.png" alt="bla" /> Temperature
          (°C): {{ Math.round(weatherToday.main.temp) }} °
        </h3>
        <p>Feels like: {{ Math.round(weatherToday.main.feels_like) }} °</p>
        <!-- <p>Lufttryck: {{ weatherToday.main.pressure }}</p> -->
        <p>
          <img class="data-img" src="@/assets/drop.png" alt="bla" /> Humidity:
          {{ weatherToday.main.humidity }} %
        </p>
        <p>
          <img class="data-img" src="@/assets/wind.png" alt="bla" /> Wind speed:
          {{ weatherToday.wind.speed }} m/s
        </p>
        <!-- <p>Sunrise: {{ weatherToday.sys.sunrise }}</p>
        <p>Sunset: {{ weatherToday.sys.sunset }}</p> -->
      </div>

      <div id="weather-today-forecast">
        <!-- <h2 class="today-forecast-header">Todays Forecast</h2> -->
        <table
          v-for="item in weatherTodayForecast.slice(0, 6)"
          :key="item.dt_txt"
        >
          <tbody class="forecast-today-wrapper">
            <tr class="box-one">
              <div class="forecast-row-left">
                <td>
                  <img
                    class="icon-img weather-icon"
                    :src="`https://openweathermap.org/img/wn/${item.weather[0].icon}@2x.png`"
                    alt="icon"
                  />
                </td>
                <td>
                  <div class="date-time">
                    <b>{{ item.dt_txt.toLocaleString().substr(11, 5) }}</b>
                  </div>
                  <div class="weather-description">
                    {{ item.weather[0].description }}
                  </div>
                </td>
              </div>
              <div class="forecast-row-right">
                <td>
                  <img class="data-img" src="@/assets/temp.png" alt="bla" />
                </td>
                <td>{{ Math.round(item.main.temp) }} °</td>
                <td>
                  <img class="data-img" src="@/assets/drop.png" alt="bla" />
                </td>
                <td>{{ item.main.humidity }} %</td>
                <td>
                  <img class="data-img" src="@/assets/wind.png" alt="bla" />
                </td>
                <td>{{ item.wind.speed }} m/s</td>
              </div>
            </tr>
            <tr>
              <div class="forecast-row-small">
                <td>
                  <img class="data-img" src="@/assets/temp.png" alt="bla" />
                </td>
                <td>{{ Math.round(item.main.temp) }} °</td>
                <td>
                  <img class="data-img" src="@/assets/drop.png" alt="bla" />
                </td>
                <td>{{ item.main.humidity }} %</td>
                <td>
                  <img class="data-img" src="@/assets/wind.png" alt="bla" />
                </td>
                <td>{{ item.wind.speed }} m/s</td>
              </div>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <div id="weather-forecast">
      <hr />
      <div class="test">
        <h2 class="forecast-headline">Forecast</h2>
        <p class="x"><a href="#weather-today">Todays Weather ↑</a></p>
      </div>

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
            <div class="forecast-row-left">
              <td>
                <img
                  class="icon-img weather-icon"
                  :src="`https://openweathermap.org/img/wn/${item.weather[0].icon}@2x.png`"
                  alt="icon"
                />
              </td>
              <td>
                <div class="date-time">
                  <b>{{ item.dt_txt.slice(0, 10) }}</b>
                </div>
                <div class="weather-description">
                  {{ item.weather[0].description }}
                </div>
              </td>
            </div>
            <div class="forecast-row-right">
              <td>
                <img class="data-img" src="@/assets/temp.png" alt="bla" />
              </td>
              <td>{{ Math.round(item.main.temp) }} °</td>
              <td>
                <img class="data-img" src="@/assets/drop.png" alt="bla" />
              </td>
              <td>{{ item.main.humidity }} %</td>
              <td>
                <img class="data-img" src="@/assets/wind.png" alt="bla" />
              </td>
              <td>{{ item.wind.speed }} m/s</td>
            </div>
          </tr>
          <tr>
            <div class="forecast-row-small">
              <td>
                <img class="data-img" src="@/assets/temp.png" alt="bla" />
              </td>
              <td>{{ Math.round(item.main.temp) }} °</td>
              <td>
                <img class="data-img" src="@/assets/drop.png" alt="bla" />
              </td>
              <td>{{ item.main.humidity }} %</td>
              <td>
                <img class="data-img" src="@/assets/wind.png" alt="bla" />
              </td>
              <td>{{ item.wind.speed }} m/s</td>
            </div>
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
              if (this.weatherTodayForecast.length < 7) {
                this.weatherTodayForecast.push(this.weatherForecast[i]);
              } else {
                break;
              }
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
              this.dayCounter < 5
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

<style scoped>
.test {
  white-space: nowrap;
  text-align: left;
}

.x {
  display: inline-block;
  border-style: solid;
  border-color: #2c3e50;
  border-width: thin;
  margin-left: 15px;
  padding-left: 0.5em;
  padding-right: 0.5em;
  padding-top: 0.1em;
  padding-bottom: 0.1em;
  border-radius: 10em;
}

a {
  text-decoration: none;
  color: #2c3e50;
}

.place {
  /* display: inline-block; */
  float: left;
  clear: left;
}

header {
  height: 90px;
  margin-left: auto;
  margin-right: auto;
  width: 100%;
}

.map-pointer {
  width: 18px;
}

.title {
  /* display: inline-block; */
  float: left;
}

.date-time {
  text-align: left;
}

.today-forecast-header {
  text-align: left;
}

.weather-description {
  text-align: left;
}

img {
  border-radius: 5em;
}

.weather-icon {
  background: rgb(216, 216, 216);
}

.today-headline {
  display: inline-block;
  text-align: left;
}

.forecast-headline {
  float: left;
}
table {
  width: 100%;
  margin-bottom: 6px;
}
/* 
tbody {
  margin-bottom: 20px;
} */

tr {
  height: 0.8em;
}

td {
  height: 1.5em;
  vertical-align: middle;
}

/* #img-now {
  width: 150px;
  padding: 0;
} */
/* #weather-forecast {
  background: lightgrey;
} */
.forecast-row-left {
  float: left;
}
/* .date-weather-cluster {
  margin-bottom: 6px;
} */
.forecast-row-right {
  float: right;
  margin-top: 6px;
}

.forecast-row-small {
  float: left;
}

.icon-img {
  width: 2.3em;
  margin-right: 20px;
}

.big-weather-icon {
  margin: 15px;
}

.data-img {
  width: 1em;
}

@media only screen and (max-width: 390px) {
  .forecast-row-right {
    visibility: collapse;
  }

  .forecast-row-small {
    visibility: visible;
  }
}

@media only screen and (min-width: 391px) {
  .forecast-row-right {
    visibility: visible;
  }

  .forecast-row-small {
    visibility: collapse;
  }
}
</style>
