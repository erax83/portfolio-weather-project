<template>
  <div id="weather-info">
    <header>
      <div id="main-title">
        <h1>Local Weather Report</h1>
      </div>
      <br />
      <div id="place">
        <p>
          <img id="map-pointer" src="@/assets/map.png" alt="map-pointer" />
          {{ weatherToday.name }}, {{ weatherToday.sys.country }}
        </p>
      </div>
    </header>
    <hr />
    <main>
      <div id="weather-today">
        <div class="sub-header">
          <h2 class="sub-headline">Todays Weather, {{ todaysDate }}</h2>
          <p class="scroll-button">
            <a href="#weather-forecast">Forecast ↓</a>
          </p>
        </div>
        <div id="weather-today-now">
          <img
            class="large-weather-icon weather-icon"
            :src="`https://openweathermap.org/img/wn/${this.weatherToday.weather[0].icon}@2x.png`"
            alt="weather-icon"
          />
          <p>{{ weatherToday.weather[0].description }}</p>
          <h3>
            <img
              class="data-img"
              src="@/assets/temp.png"
              alt="thermometer-icon"
            />
            Temperature (°C): {{ Math.round(weatherToday.main.temp) }} °
          </h3>
          <p>Feels like: {{ Math.round(weatherToday.main.feels_like) }} °</p>
          <p>
            <img class="data-img" src="@/assets/drop.png" alt="drop-icon" />
            Humidity: {{ weatherToday.main.humidity }} %
          </p>
          <p>
            <img class="data-img" src="@/assets/wind.png" alt="wind-icon" />
            Wind speed: {{ weatherToday.wind.speed }} m/s
          </p>
        </div>

        <div id="weather-today-forecast">
          <table
            v-for="item in weatherTodayForecast.slice(0, 6)"
            :key="item.dt_txt"
          >
            <tbody>
              <tr>
                <div class="forecast-row-left">
                  <td>
                    <img
                      class="icon-img weather-icon"
                      :src="`https://openweathermap.org/img/wn/${item.weather[0].icon}@2x.png`"
                      alt="weather-icon"
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
                    <img
                      class="data-img"
                      src="@/assets/temp.png"
                      alt="thermometer-icon"
                    />
                  </td>
                  <td>{{ Math.round(item.main.temp) }} °</td>
                  <td>
                    <img
                      class="data-img"
                      src="@/assets/drop.png"
                      alt="drop-icon"
                    />
                  </td>
                  <td>{{ item.main.humidity }} %</td>
                  <td>
                    <img
                      class="data-img"
                      src="@/assets/wind.png"
                      alt="wind-icon"
                    />
                  </td>
                  <td>{{ item.wind.speed }} m/s</td>
                </div>
              </tr>
              <tr>
                <div class="forecast-row-small-screen">
                  <td>
                    <img
                      class="data-img"
                      src="@/assets/temp.png"
                      alt="thermometer-icon"
                    />
                  </td>
                  <td>{{ Math.round(item.main.temp) }} °</td>
                  <td>
                    <img
                      class="data-img"
                      src="@/assets/drop.png"
                      alt="drop-icon"
                    />
                  </td>
                  <td>{{ item.main.humidity }} %</td>
                  <td>
                    <img
                      class="data-img"
                      src="@/assets/wind.png"
                      alt="wind-icon"
                    />
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
        <div class="sub-header">
          <h2 class="sub-headline">Forecast</h2>
          <p class="scroll-button">
            <a href="#weather-today">Todays Weather ↑</a>
          </p>
        </div>
        <table v-for="item in weatherForecastFiltered" :key="item.dt_txt">
          <tbody v-if="item.dt_txt.slice(11, 20) == '15:00:00'">
            <tr>
              <div class="forecast-row-left">
                <td>
                  <img
                    class="icon-img weather-icon"
                    :src="`https://openweathermap.org/img/wn/${item.weather[0].icon}@2x.png`"
                    alt="weather-icon"
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
                  <img
                    class="data-img"
                    src="@/assets/temp.png"
                    alt="thermometer-icon"
                  />
                </td>
                <td>{{ Math.round(item.main.temp) }} °</td>
                <td>
                  <img
                    class="data-img"
                    src="@/assets/drop.png"
                    alt="drop-icon"
                  />
                </td>
                <td>{{ item.main.humidity }} %</td>
                <td>
                  <img
                    class="data-img"
                    src="@/assets/wind.png"
                    alt="wind-icon"
                  />
                </td>
                <td>{{ item.wind.speed }} m/s</td>
              </div>
            </tr>
            <tr>
              <div class="forecast-row-small-screen">
                <td>
                  <img
                    class="data-img"
                    src="@/assets/temp.png"
                    alt="thermometer-icon"
                  />
                </td>
                <td>{{ Math.round(item.main.temp) }} °</td>
                <td>
                  <img
                    class="data-img"
                    src="@/assets/drop.png"
                    alt="drop-icon"
                  />
                </td>
                <td>{{ item.main.humidity }} %</td>
                <td>
                  <img
                    class="data-img"
                    src="@/assets/wind.png"
                    alt="wind-icon"
                  />
                </td>
                <td>{{ item.wind.speed }} m/s</td>
              </div>
            </tr>
          </tbody>
        </table>
      </div>
    </main>
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
/* Parts of page */
header {
  height: 90px;
  margin-left: auto;
  margin-right: auto;
  width: 100%;
}

/* Images */
img {
  border-radius: 5em;
}

.icon-img {
  width: 2.3em;
  margin-right: 20px;
}

#map-pointer {
  width: 18px;
}

.weather-icon {
  background: rgb(216, 216, 216);
}

.large-weather-icon {
  margin: 15px;
}

.data-img {
  width: 1em;
}

/* Links */
a {
  text-decoration: none;
  color: #2c3e50;
}

/* Buttons */
.scroll-button {
  display: inline-block;
  border-style: solid;
  border-color: #2c3e50;
  border-width: thin;
  margin-left: 15px;
  padding-left: 0.5em;
  padding-right: 0.5em;
  padding-top: 0.2em;
  padding-bottom: 0.1em;
  border-radius: 10em;
}

/* Headlines and data */
#main-title {
  float: left;
}

.sub-header {
  white-space: nowrap;
  text-align: left;
}

.sub-headline {
  display: inline-block;
  text-align: left;
}

.today-forecast-header {
  text-align: left;
}

#place {
  float: left;
  clear: left;
}

.date-time {
  text-align: left;
}

.weather-description {
  text-align: left;
}

/* Table */
table {
  width: 100%;
  margin-bottom: 6px;
}

tr {
  height: 0.8em;
}

td {
  height: 1.5em;
  vertical-align: middle;
}

.forecast-row-left {
  float: left;
}

.forecast-row-right {
  float: right;
  margin-top: 6px;
}

.forecast-row-small {
  float: left;
}

/* Responsive design */
@media only screen and (max-width: 390px) {
  .forecast-row-right {
    visibility: collapse;
  }

  .forecast-row-small-screen {
    visibility: visible;
  }
}

@media only screen and (min-width: 391px) {
  .forecast-row-right {
    visibility: visible;
  }

  .forecast-row-small-screen {
    visibility: collapse;
  }
}
</style>
