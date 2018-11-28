<template>
  <div class="weatherApp rounded m-5">
    <!-- <transition> -->
      <div class="location border rounded p-3 bg-light" v-if="!results">
        <h2 class="text-center my-2">Weather Widget</h2>
        <form @submit.prevent="getResults(location, units)">
          <div class="form-group my-3">
            <input type="text" class="form-control" v-model="location" placeholder="Location">
          </div>
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
          <button type="submit" class="btn btn-success my-4">Get weather</button>
        </form>
      </div>
    <!-- </transition>
    <transition> -->
      <div class="viewWeather" v-if="results">
        <div class="top bg-success text-light rounded-top">
          <div class="row p-2">
            <div class="col text-left">
              <h4>Location</h4>
            </div>
            <div class="col text-right">
              <h5>Time</h5>
            </div>
            <div class="col text-right close" @click="results=''">
              <h5 class="text-white">Close</h5>
            </div>
          </div>
        </div>
        <div class="content" :class="[ night ? 'bg-dark text-light' : 'bg-light text-dark']">
          <div class="row p-2">
            <div class="col">

            </div>
            <div class="col">

            </div>
          </div>
          <div class="row p-2">
            <div class="col">

            </div>
            <div class="col">

            </div>
          </div>
          <div class="row p-2">
            <div class="col">

            </div>
            <div class="col">

            </div>
          </div>
        </div>
        <div class="footer bg-info text-light rounded-bottom p-2">
          <h4 class="text-right">Footer</h4>
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
      results: '',
      units: 'metric',
      location: '',
      night: true
    }
  },
  methods: {
    getResults (location, units) {
      axios.get('https://api.openweathermap.org/data/2.5/weather?q=' + location + '&units=' + units + '&APPID=055f1d666fd988c72b7af102a40c00a8')
        .then(response => {
          console.log(response.data)
          this.results = response.data
        })
    }
  }
}
</script>

<style lang="sass">

.content

.weatherApp
  width: 300px
  .top
  .footer
.close
  cursor: pointer

.v-enter-active, .v-leave-active
  transition: .5s
.v-enter, .v-leave-to
  opacity: 0
</style>
