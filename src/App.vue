<template>
  <div id="app">
    <h2>{{timezone.timezone}}</h2>
    <WeatherDash v-if="currentWeather" :currentWeather="currentWeather" />
    <WeeklyForecast v-if="currentWeather" :dailyWeather="dailyWeather" :currentWeather="currentWeather" />
    <div v-else>
      <h3>No Weather</h3>
    </div>
  </div>
</template>

<script>
  import axios from 'axios'
  import WeatherDash from './components/WeatherDash.vue'
  import WeeklyForecast from './components/WeeklyForecast.vue'
const API_KEY = "66dfc8b58a437fe6c4ec7910956e26f5"
  export default {
    name: 'App',
    components: {
      WeatherDash,
      WeeklyForecast
    },
    data: () => ({
      dailyWeather: [],
      currentWeather: null,
      timezone:[]
    }),
    mounted: function() {
      navigator.geolocation.getCurrentPosition(async (position) => {
        await this.getCurrentWeather(position.coords)
      })
    },
    methods: {
      async getCurrentWeather(coords){
        const res = await axios.get(`https://api.openweathermap.org/data/2.5/onecall?lat=${coords.latitude}&lon=${coords.longitude}&units=imperial&appid=${API_KEY}`)
        this.currentWeather = res.data.current
        this.timezone = res.data
        res.data.daily.splice(0,1)
        this.dailyWeather = res.data.daily
      }
    }
  }
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  html{
      background-color: #E4BCA4;
  }

</style>

