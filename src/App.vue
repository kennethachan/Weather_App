<template>
  <div id="app">
    <h1 class="timezone">
      <span class="where" :style="{ color: isDaytime ? '#ec6e4c' : 'white' }">
        {{ formattedTimezone }}
      </span>
    </h1>
    <WeatherDash v-if="currentWeather" :currentWeather="currentWeather" :isDaytime="isDaytime" />
    <WeeklyForecast v-if="currentWeather" :dailyWeather="dailyWeather" :currentWeather="currentWeather" :isDaytime="isDaytime" />
    <div v-else>
      <h3 class="no-weather">Loading...</h3>
    </div>
  </div>
</template>

<script>
  import axios from 'axios'
  import WeatherDash from './components/WeatherDash.vue'
  import WeeklyForecast from './components/WeeklyForecast.vue'

  const API_KEY = "e147b2263e695fb6a2921aa16fd3615c"

  export default {
    name: 'App',
    components: {
      WeatherDash,
      WeeklyForecast
    },
    data: () => ({
      dailyWeather: [],
      currentWeather: null,
      timezone: ""
    }),
    computed: {
      formattedTimezone() {
        if (!this.timezone) return "";
        const parts = this.timezone.split("/");
        return parts.length > 1 ? parts[1].replace(/_/g, " ") : this.timezone;
      },
      isDaytime() {
        if (!this.currentWeather) return true; // Default to daytime if no weather data
        const now = Math.floor(Date.now() / 1000); // Get current time in UNIX timestamp
        return now >= this.currentWeather.sunrise && now < this.currentWeather.sunset;
      }
    },
    watch: {
      isDaytime(newVal) {
        document.documentElement.style.backgroundColor = newVal ? "rgb(208, 226, 255)" : "#132f42";
      }
    },
    mounted() {
      navigator.geolocation.getCurrentPosition(async (position) => {
        await this.getCurrentWeather(position.coords);
      });
    },
    methods: {
      async getCurrentWeather(coords) {
        try {
          const res = await axios.get(
            `https://api.openweathermap.org/data/3.0/onecall?lat=${coords.latitude}&lon=${coords.longitude}&units=imperial&appid=${API_KEY}`
          );
          console.log(res.data);
          this.currentWeather = res.data.current;
          this.timezone = res.data.timezone || ""; // Ensure a valid string is assigned
          res.data.daily.splice(0, 1);
          this.dailyWeather = res.data.daily;

          // Set the background color initially
          document.documentElement.style.backgroundColor = this.isDaytime ? "rgb(208, 226, 255)" : "#132f42";

        } catch (error) {
          console.error("Error fetching weather data:", error);
        }
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

  .timezone {
    text-align: right;
    position: relative;
    font-size: 75px;
  }

  .where {
    font-weight: 900;
  }

  .no-weather {
     color: #B13E19;
  }

  .heading {
    color: #17539C;
  }
</style>