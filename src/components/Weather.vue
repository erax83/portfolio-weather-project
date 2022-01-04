<template>
  <div class="weather-info">
    <div class="header-info">
      <!-- <img src="@/assets/headericon.png" alt="symbol"> -->
      <div>
        <h1 class="title">Local Weather Report</h1>
      </div>

      <div class="place">
        <p>
          <img src="@/assets/map.png" alt="map-pointer" class="map-pointer" />
          {{ weatherToday.name }}, {{ weatherToday.sys.country }}
        </p>
      </div>
    </div>
    <hr>
    <div id="weather-today">
      <h2 class="today-headline">Todays Weather, {{ todaysDate }}</h2>
      <div id="weather-today-right-now">
        <img
          id="img-now"
          :src="`https://openweathermap.org/img/wn/${this.weatherToday.weather[0].icon}@2x.png`"
          alt="weathericon"
        />
        <!-- <p>{{ lat }}, {{ long }}</p> -->
        <p>{{ weatherToday.weather[0].description }}</p>
        <h1>Temperature (°C): {{ Math.round(weatherToday.main.temp) }} °</h1>
        <p>Feels like: {{ Math.round(weatherToday.main.feels_like) }} °</p>
        <!-- <p>Lufttryck: {{ weatherToday.main.pressure }}</p> -->
        <p>Humidity: {{ weatherToday.main.humidity }} %</p>
        <p>Wind speed: {{ weatherToday.wind.speed }} m/s</p>
        <p>Sunrise: {{ weatherToday.sys.sunrise }}</p>
        <p>Sunset: {{ weatherToday.sys.sunset }}</p>
      </div>

      <div id="weather-today-forecast">
        <!-- <h2 class="today-forecast-header">Todays Forecast</h2> -->
        <table
          id="forecast-today-list"
          v-for="item in weatherTodayForecast.slice(0, 6)"
          :key="item.dt_txt"
        >
          <tbody class="forecast-today-wrapper">
            <tr class="box-one">
              <div class="forecast-row-left">
                <td>
                  <img
                    class="icon-img"
                    :src="`https://openweathermap.org/img/wn/${item.weather[0].icon}@2x.png`"
                    alt="icon"
                  />
                </td>
                <td>
                  <div class="date-time">
                    <b>{{ item.dt_txt.toLocaleString().substr(11, 5) }}</b>
                  </div>
                  <div>{{ item.weather[0].description }}</div>
                </td>
              </div>
              <div class="forecast-row-right">
                <td>
                  <img class="data-img" src="@/assets/celsius.png" alt="bla" />
                </td>
                <td>{{ Math.round(item.main.temp) }} °</td>
                <td>
                  <img
                    class="data-img"
                    src="@/assets/blood-drop.png"
                    alt="bla"
                  />
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
      <h2 class="forecast-headline">Forecast</h2>
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
                  class="icon-img"
                  :src="`https://openweathermap.org/img/wn/${item.weather[0].icon}@2x.png`"
                  alt="icon"
                />
              </td>
              <td>
                <div class="date-time">
                  <b>{{ item.dt_txt.slice(0, 10) }}</b>
                </div>
                <div>{{ item.weather[0].description }}</div>
              </td>
            </div>
            <div class="forecast-row-right">
              <td>
                <img class="data-img" src="@/assets/celsius.png" alt="bla" />
              </td>
              <td>{{ Math.round(item.main.temp) }} °</td>
              <td>
                <img class="data-img" src="@/assets/blood-drop.png" alt="bla" />
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

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.header-info {
  height: 60px;
  margin-left: auto;
  margin-right: auto;
  width: 100%;
}

.map-pointer {
  width: 18px;
}

.title {
  float: left;
}

.place {
  float: right;
}

.date-time {
  text-align: left;
}

.today-forecast-header {
  text-align: left;
}

img {
  /* background: lightgray; */
  border-radius: 5em;
}

.today-headline {
  text-align: left;
}

.forecast-headline {
  float: left;
}
table {
  width: 100%;
}

td {
  height: 3.5em;
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

.icon-img {
  width: 2.3em;
  margin-right: 20px;
}

.data-img {
  width: 1em;
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
