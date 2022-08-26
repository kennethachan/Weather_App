<template>
  <section class="current">
    <h3 class="heading">Right Now</h3>
    <div>
  <table>
  <thead>
    <tr>
       <td> <p class="dayy">{{ generateDay(currentWeather.dt)  }}</p><p>{{ generateDate(currentWeather.dt) }}</p></td>
      <td>    <img class="pic" :src="generateIconUrl(currentWeather.weather[0].icon)" /></td>
            <td><p class="">{{Math.ceil(currentWeather.feels_like)  }}&#176;</p></td>
      <td> <p><span class="bold">Humidity:</span> {{ currentWeather.humidity}}%</p></td>
            <td><p><span class="bold">Wind Speed: </span>{{ Math.ceil(currentWeather.wind_speed)}}mph</p></td>
      <td><p><span class="bold">UV Index:</span> <br/>{{ Math.ceil(currentWeather.uvi)}}</p></td>
    </tr>
  </thead>
</table>
    </div>
  </section>
</template>

<script>
  export default {
    name: 'WeatherDash',
    props: ['currentWeather'],
    methods: {
      generateIconUrl(ico) {
        return `http://openweathermap.org/img/wn/${ico}@2x.png`
      },
      generateDate(dt) {
        let dateTime = new Date(dt * 1000)
        let dateStr = dateTime.toDateString().split(' ')
        dateStr.splice(0, 1)
        dateStr.splice(dateStr.length - 1)
        return dateStr.join(' ')
      },
      generateDay(dt) {
        const days = [
          "Sunday",
          "Monday",
          "Tuesday",
          "Wednesday",
          "Thursday",
          "Friday",
          "Saturday"
        ]
        let date = new Date(dt * 1000)
        let day = date.getDay()
        return days[day]
      },
    }
  }
</script>

<style>
  .current {
    margin: 2px;
    width: 700px;
    border-radius: 15px;
    margin: 20px;
  }
</style>