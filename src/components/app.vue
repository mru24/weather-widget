<template>
  <div class="weatherApp rounded">
    <!-- <transition name="searchBox"> -->
      <div class="container" v-if="!currentWeather">
        <div class="location border rounded p-3 bg-light">
          <h2 class="text-center my-2">Weather Widget</h2>
          <form @submit.prevent="getResults(query, units), getForecast(query, units)">
            <div class="form-group my-3">
              <input type="text" class="form-control" v-model="query" placeholder="Location">
            </div>
            Units:
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
            </div>
            <div class="row">
              <div class="col text-center">
                <button type="submit" class="btn btn-success my-4">Get weather</button>
              </div>
            </div>
          </form>
        </div>
      </div>
    <!-- </transition>

    <transition name="searchBox"> -->
      <div class="container" v-if="currentWeather">
        <div class="viewWeather">
          <div class="top bg-success text-light rounded-top">
            <div class="row p-2">
              <div class="col text-left">
                <h4>{{ location }}</h4>
              </div>
              <div class="col text-right close" @click="currentWeather=false">
                <h5 class="text-white">Close</h5>
              </div>
            </div>
            <div class="row">
              <div class="col text-left mx-2">
                <p>{{ currentTime }}</p>
              </div>
            </div>
          </div>
          <div class="content text-center" :class="[ night ? 'bg-dark text-light' : 'bg-light text-dark']">
            <div class="row p-2">
              <div class="col">
                <div class="row">
                  <div class="col">
                    <h1 class="display-5">{{ currentTemp | temperature }} &#8451;</h1>
                  </div>
                </div>
                <div class="row">
                  <div class="col">
                    <p><small>min: </small>{{ minTemp | temperature }}</p>
                  </div>
                  <div class="col">
                    <p><small>max: </small>{{ maxTemp | temperature }}</p>
                  </div>
                </div>
              </div>
              <div class="col">
                <div class="row">
                  <div class="col">
                    <img src="../assets/sunrise.png" alt="" width="30">
                    {{ sunrise }}
                  </div>
                </div>
                <div class="row">
                  <div class="col">
                    <img src="../assets/sunset.jpg" alt="" width="30">
                    {{ sunset }}
                  </div>
                </div>
              </div>
            </div>
            <div class="row p-2 text-capitalize">
              <div class="col">
                <div class="row">
                  <div class="col">
                    <h6>{{ overcast }}</h6>
                  </div>
                </div>
                <div class="row">
                  <div class="col">
                    <img :src="icon" alt="" width="70">
                  </div>
                </div>
                <div class="row">
                  <div class="col">

                  </div>
                </div>
              </div>
            </div>
            <div class="row p-2">
              <div class="col">
                Wind: {{ wind }}
              </div>
              <div class="col">
                Pressure: {{ pressure }}
              </div>
              <div class="col">
                Humidity: {{ humidity }}
              </div>
            </div>
          </div>
          <!-- <div class="footer bg-info text-light rounded-bottom p-2">
            <h4 class="text-right">Footer</h4>
          </div> -->
          <div class="accordion" id="forecastAccordion">
            <div class="card">
              <div class="card-header" id="headingOne">
                <h5 class="mb-0">
                  <button class="btn btn-link" type="button" data-toggle="collapse" data-target="#collapseOne" aria-expanded="true" aria-controls="collapseOne">
                    5 days forecast
                  </button>
                </h5>
              </div>

              <div id="collapseOne" class="collapse hide" aria-labelledby="headingOne" data-parent="#forecastAccordion">
                <div class="card" v-for="(forecast, index) in Forecast" :key="index">
                  <div class="card-header">
                    {{ forecast.dt | time }}
                  </div>
                  <div class="card-body">
                    <div class="row">
                      <div class="col text-center align-middle">
                        <small>Temperature</small><br>
                        <h3 class="">{{ forecast.main.temp | temperature }} &#8451;</h3>
                      </div>
                      <div class="col text-left">
                        <div class="row">
                          <div class="col">
                            <small>max: </small>{{ forecast.main.temp_max | temperature }}
                          </div>
                        </div>
                        <div class="row">
                          <div class="col">
                            <small>min: </small>{{ forecast.main.temp_min | temperature }}
                          </div>
                        </div>
                      </div>
                    </div>
                    <div class="row p-2 text-capitalize text-center">
                      <div class="col">
                        <div class="row">
                          <div class="col">
                            <h6>{{ forecast.weather[0].description }}</h6>
                          </div>
                        </div>
                        <div class="row">
                          <div class="col">
                            <img :src="'http://openweathermap.org/img/w/' + forecast.weather[0].icon + '.png'" alt="" width="40">
                          </div>
                        </div>
                        <div class="row">
                          <div class="col">

                          </div>
                        </div>
                      </div>
                    </div>
                    <div class="row p-2">
                      <div class="col">
                        Wind: {{ forecast.wind.speed }} m/s
                      </div>
                      <div class="col">
                        Pressure: {{ forecast.main.pressure }} kPa
                      </div>
                      <div class="col">
                        Humidity: {{ forecast.main.humidity }} %
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    <!-- </transition> -->
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      query: '',
      units: 'metric',
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
      forecast: '',
      date: '',
      forecastWeather: false,
      Forecast: '',
      F_Temp: '',
      F_minTemp: '',
      F_maxTemp: '',
      F_pressure: '',
      F_humidity: '',
      F_wind: '',
      F_overcast: '',
      F_icon: '',
      F_time: '',
      F_sunrise: '',
      F_sunset: '',
      F_forecast: '',
      F_date: '',
      night: false
    }
  },
  methods: {
    getResults (query, units) {
      axios.get('https://api.openweathermap.org/data/2.5/weather?q=' + query + '&units=' + units + '&APPID=055f1d666fd988c72b7af102a40c00a8')
        .then(response => {
          console.log(response.data)
          this.currentWeather = true
          this.location = response.data.name
          this.currentTemp = response.data.main.temp
          this.minTemp = response.data.main.temp_min
          this.maxTemp = response.data.main.temp_max
          this.pressure = response.data.main.pressure
          this.humidity = response.data.main.humidity + '%'
          this.wind = response.data.wind.speed + 'm/s'
          this.overcast = response.data.weather[0].description
          this.icon = 'http://openweathermap.org/img/w/' + response.data.weather[0].icon + '.png'
          this.time = new Date(response.data.dt * 1000).toLocaleTimeString('en-GB').slice(0, 5)
          this.sunrise = new Date(response.data.sys.sunrise * 1000).toLocaleTimeString('en-GB').slice(0, 5)
          this.sunset = new Date(response.data.sys.sunset * 1000).toLocaleTimeString('en-GB').slice(0, 5)
        })
    },
    getForecast (query, units) {
      axios.get('https://api.openweathermap.org/data/2.5/forecast?q=' + query + '&units=' + units + '&APPID=055f1d666fd988c72b7af102a40c00a8')
        .then(response => {
          console.log(response.data.list)
          this.Forecast = response.data.list
          this.forecastWeather = true
        })
    },
    getDate () {
      let d = new Date()
      var days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']
      let day = days[d.getDay()]
      let D = d.getDate()
      let M = 1 + d.getMonth()
      let Y = d.getFullYear()
      let h = d.getHours()
      let m = d.getMinutes()
      if (m > 0 && m < 9) {
        m = 0 + '' + m
      }
      let s = d.getSeconds()
      if (s > 0 && s < 9) {
        s = 0 + '' + s
      }
      return day + '  ' + D + '/' + M + '/' + Y + '  ' + h + ':' + m + ':' + s
    }
  },
  filters: {
    temperature: (value) => {
      return `${value.toFixed(0)}`
    },
    time: function (value) {
      if (value) {
        return new Date(value * 1000).toLocaleTimeString('en-GB').slice(0, 5)
      }
    }
  },
  mounted: function () {
    setInterval(() => {
      this.currentTime = this.getDate()
    }, 1000)
  }
}
</script>

<style lang="sass">

.container
  // position: fixed
  // width: inherit
li
  list-style: none

.weatherApp
  width: 340px
  height: auto
  margin: 30px
  @media only screen and (max-width: 400px)
    width: auto
    margin: 5px
  .top
  .footer
.close
  cursor: pointer
.descr
  text-transform: capitalize
// .searchBox-leave-active
//   transition: .5s
// .searchBox-enter-active
//   transition: .5s
//   transition-delay: .5s
// .searchBox-leave-to
//   transform: scale3D(0, .6, .3)
// .searchBox-enter
//   transform: scale3D(0, .6, .3)
//
// .resultBox-enter-active, .resultBox-leave-active
//   transition: .5s
// .resultBox-leave-to
//   transform: translateX(-100%)
// .resultBox-enter
//   transform: translateX(100%)

</style>
