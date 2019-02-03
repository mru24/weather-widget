<template>
  <div class="weatherApp">
    <div class="row location" v-if="!currentWeather">
      <div class="col border bg-light p-3 rounded shadow">
        <h3 class="text-center my-3">Weather Widget</h3>
        <form @submit.prevent="getWeather(query, units), getForecast(query, units)">
          <div class="form-group my-4">
            <div class="locDiv">
              <input type="text" class="form-control locInput" v-model="query" placeholder="Location">
              <button class="locButton link" @click.prevent="geoLocation">
                <img src="static/location.png" alt="" height="30">
              </button>
            </div>
          </div>
          <!-- Units:
          <div class="form-check">
            <input class="form-check-input" type="radio" name="exampleRadios" id="Radio1" v-model="units" value="metric" checked>
            <label class="form-check-label" for="Radio1">
              Metric
            </label>
          </div>
          <div class="form-check">
            <input class="form-check-input" type="radio" name="exampleRadios" id="Radio2" v-model="units" value="imperial">
            <label class="form-check-label" for="Radio2">
              Imperial
            </label>
          </div>
          <div class="form-check">
            <input class="form-check-input" type="radio" name="exampleRadios" id="Radio3" v-model="units" value="">
            <label class="form-check-label" for="Radio3">
              Kelvin
            </label>
          </div> -->
          <div class="row">
            <div class="col text-center">
              <button type="submit" class="btn btn-success my-4">Get weather</button>
            </div>
          </div>
        </form>
      </div>
    </div>

    <div class="row viewWeather" v-if="currentWeather">
      <div class="col shadow">
        <div class="row bg-success text-light rounded-top p-2">
          <div class="col">
            <div class="row">
              <div class="col text-left">
                <h4>{{ location }}</h4>
              </div>
              <div class="col text-right link" @click="currentWeather=false">
                <h5 class="text-white">Close</h5>
              </div>
            </div>
            <div class="row">
              <div class="col">
                <p class="lead">{{ currentDay }} {{ currentDate }}</p>
              </div>
              <div class="col">
                <p class="lead text-right">{{ currentTime }}</p>
              </div>
            </div>
            <div class="row">
              <div class="col">
                <h4 class="lead text-right link" @click="forecastWeather=!forecastWeather">
                  <span v-if="!forecastWeather">
                    5 day forecast
                  </span>
                  <span v-else>
                    Today
                  </span>
                </h4>
              </div>
            </div>
          </div>
        </div>
        <!-- CONTENT DIV -->
        <transition mode="out-in">
          <div class="row p-2" :class="[ night ? 'bg-dark text-light' : 'bg-light text-dark']" v-if="!forecastWeather">
            <div class="col">
              <div class="row mb-4">
                <div class="col-8">
                  <h1 class="display-3 text-center">{{ currentTemp | fixed }} &#8451;</h1>
                </div>
                <div class="col-4">
                  <div class="row">
                    <div class="col">
                      <p><small>min: </small>{{ minTemp | fixed }} &#8451;</p>
                    </div>
                  </div>
                  <div class="row">
                    <div class="col">
                      <p><small>max: </small>{{ maxTemp | fixed }} &#8451;</p>
                    </div>
                  </div>
                </div>
              </div>
              <div class="row p-2 text-capitalize text-center">
                <div class="col">
                  <div class="row">
                    <div class="col">
                      <h6 class="lead">{{ overcast }}</h6>
                    </div>
                  </div>
                  <div class="row">
                    <div class="col m-3">
                      <img :src="icon" alt="" width="110">
                    </div>
                  </div>
                </div>
              </div>
              <div class="row p-2 text-center">
                <div class="col">
                  <p>Wind:</p>
                  <p>{{ wind | fixed }} m/s</p>
                </div>
                <div class="col">
                  <p>Pressure:</p>
                  <p>{{ pressure | fixed }} hPa</p>
                </div>
                <div class="col">
                  <p>Humidity:</p>
                  <p>{{ humidity | fixed }} %</p>
                </div>
              </div>
            </div>
          </div>
        </transition>
        <!-- FORECAST -->
        <transition>
          <div class="row" :class="[ night ? 'bg-dark text-light' : 'bg-light text-dark']" v-if="forecastWeather">
            <div class="col">
              <div class="row" v-for="(item, index) in Forecast" :key="index">
                <div class="col">
                  <div class="row p-1">
                    <span class="text-center">{{ item.dt_txt.slice(0, 10) }}</span>
                  </div>
                  <div class="row p-1 border-bottom">
                    <div class="col text-center">
                      <div class="row">
                        <div class="col">
                          <div class="row">
                            <img class="d-block mx-auto" :src="'http://openweathermap.org/img/w/' + item.weather[0].icon + '.png'" alt="" width="40">
                          </div>
                          <div class="row">
                            <small class="text-center w-100">
                              <span class="text-capitalize">{{ item.weather[0].description }}</span>
                            </small>
                          </div>
                        </div>
                      </div>
                    </div>
                    <div class="col">
                      <div class="row">
                        <span class="align-center">
                          <p class="lead">{{ item.main.temp_min | fixed }} &#8451; / {{ item.main.temp_max | fixed }} &#8451;</p>
                        </span>
                      </div>
                    </div>
                    <div class="col">
                      <div class="row">
                        <div class="col">
                          <div class="row">
                            Wind: {{ item.wind.speed | fixed }} m/s
                          </div>
                          <div class="row">
                            Press: {{ item.main.pressure | fixed }} hPa
                          </div>
                          <div class="row">
                            Humid: {{ item.main.humidity | fixed }} %
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </transition>

        <!-- <div class="row bg-success text-light rounded-bottom p-2">
          <div class="col">
            <h4 class="lead text-right" @click="forecastWeather=!forecastWeather">5 day forecast</h4>
          </div>
        </div> -->
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      query: '',
      units: 'metric',
      currentDay: '',
      currentDate: '',
      currentTime: '',
      currentWeather: false,
      location: '',
      currentTemp: '',
      minTemp: '',
      maxTemp: '',
      pressure: '',
      humidity: '',
      wind: '',
      overcast: '',
      icon: '',
      time: '',
      sunrise: '',
      sunset: '',
      date: '',
      forecastWeather: false,
      Forecast: [],
      night: ''
    }
  },
  methods: {
    getWeather (query, units) {
      axios.get('https://api.openweathermap.org/data/2.5/weather?q=' + query + '&units=' + units + '&APPID=055f1d666fd988c72b7af102a40c00a8')
        .then(response => {
          console.log(response.data)
          this.currentWeather = true
          this.location = response.data.name
          this.currentTemp = response.data.main.temp
          this.minTemp = response.data.main.temp_min
          this.maxTemp = response.data.main.temp_max
          this.pressure = response.data.main.pressure
          this.humidity = response.data.main.humidity
          this.wind = response.data.wind.speed
          this.overcast = response.data.weather[0].description
          this.icon = 'http://openweathermap.org/img/w/' + response.data.weather[0].icon + '.png'
          this.time = new Date(response.data.dt * 1000).toLocaleTimeString('en-GB').slice(0, 5)
          this.sunrise = new Date(response.data.sys.sunrise * 1000).toLocaleTimeString('en-GB').slice(0, 5)
          this.sunset = new Date(response.data.sys.sunset * 1000).toLocaleTimeString('en-GB').slice(0, 5)
        })
    },
    getWeatherByLoc (position, units) {
      let lat = position.coords.latitude
      let lon = position.coords.longitude
      axios.get('https://api.openweathermap.org/data/2.5/weather?lat=' + lat + '&lon=' + lon + '&units=' + units + '&APPID=055f1d666fd988c72b7af102a40c00a8')
        .then(response => {
          console.log(response.data)
          this.currentWeather = true
          this.location = response.data.name
          this.currentTemp = response.data.main.temp
          this.minTemp = response.data.main.temp_min
          this.maxTemp = response.data.main.temp_max
          this.pressure = response.data.main.pressure
          this.humidity = response.data.main.humidity
          this.wind = response.data.wind.speed
          this.overcast = response.data.weather[0].description
          this.icon = 'http://openweathermap.org/img/w/' + response.data.weather[0].icon + '.png'
          this.time = new Date(response.data.dt * 1000).toLocaleTimeString('en-GB').slice(0, 5)
          this.sunrise = new Date(response.data.sys.sunrise * 1000).toLocaleTimeString('en-GB').slice(0, 5)
          this.sunset = new Date(response.data.sys.sunset * 1000).toLocaleTimeString('en-GB').slice(0, 5)
        })
    },
    getForecast (query, units) {
      this.Forecast = []
      axios.get('https://api.openweathermap.org/data/2.5/forecast?q=' + query + '&units=' + units + '&APPID=055f1d666fd988c72b7af102a40c00a8')
        .then(response => {
          console.log(response.data.list)
          for (let i = 0; i < response.data.list.length; i += 8) {
            this.Forecast.push(response.data.list[i])
          }
        })
    },
    getForecastByLoc (position, units) {
      let lat = position.coords.latitude
      let lon = position.coords.longitude
      this.Forecast = []
      axios.get('https://api.openweathermap.org/data/2.5/forecast?lat=' + lat + '&lon=' + lon + '&units=' + units + '&APPID=055f1d666fd988c72b7af102a40c00a8')
        .then(response => {
          console.log(response.data.list)
          for (let i = 0; i < response.data.list.length; i += 8) {
            this.Forecast.push(response.data.list[i])
          }
        })
    },
    getDay () {
      let d = new Date()
      var days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']
      let day = days[d.getDay()]
      return day
    },
    getDate () {
      let d = new Date()
      let D = d.getDate()
      let M = 1 + d.getMonth()
      let Y = d.getFullYear()
      return D + '/' + M + '/' + Y
    },
    getTime () {
      let d = new Date()
      let h = d.getHours()
      if (h > 6 && h < 19) {
        this.night = false
      } else {
        this.night = true
      }
      let m = d.getMinutes()
      if (m >= 0 && m <= 9) {
        m = 0 + '' + m
      }
      let s = d.getSeconds()
      if (s >= 0 && s <= 9) {
        s = 0 + '' + s
      }
      return h + ':' + m
    },
    geoLocation () {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          (position) => {
            console.log(position)
            this.getWeatherByLoc(position, this.units)
            this.getForecastByLoc(position, this.units)
          }, (error) => {
            console.log(error)
          }
        )
      }
    }
  },
  filters: {
    fixed: (value) => {
      return `${value.toFixed(0)}`
    },
    time: function (value) {
      if (value) {
        return new Date(value * 1000).toLocaleTimeString('en-GB').slice(0, 5)
      }
    }
  },
  mounted () {
    setInterval(() => {
      this.currentDay = this.getDay()
      this.currentDate = this.getDate()
      this.currentTime = this.getTime()
    }, 1000)
  }
}
</script>

<style lang="sass">

li
  list-style: none

.weatherApp
  width: 360px
  height: auto
  margin: 30px
  @media only screen and (max-width: 600px)
    width: 92%
    margin: 2px 4%
.link
  cursor: pointer
  &:hover
    text-decoration: underline

.locDiv
  position: relative
  .locButton
    position: absolute
    top: 0
    right: 0
    height: 100%

.v-enter-active, .v-leave-active
  position: fixed
  width: 390px
  height: inherit
  transform-origin: top
  transition: all 1s
.v-enter, .v-leave-to
  transform: scale3d(1,0,1)
  opacity: 0

</style>
