<template>
  <section class="current">
    <p class="today" :style="{ color: isDaytime ? '#48484a' : 'white' }">Now</p>
    <div>
      <table>
        <thead>
          <tr>
            <td>
              <p class="dayy">{{ generateDay(currentWeather.dt) }}</p>
              <p>{{ generateDate(currentWeather.dt) }}</p>
            </td>
            <td>
              <img class="pic" :src="generateIconUrl(currentWeather.weather[0].icon)" />
            </td>
            <td>
              <p><span class="bold">Real Feel: </span>{{ Math.ceil(currentWeather.feels_like) }}&#176;</p>
            </td>
            <td>
              <p><span class="bold">Humidity:</span> {{ currentWeather.humidity }}%</p>
            </td>
            <td>
              <p><span class="bold">Wind Speed: </span>{{ Math.ceil(currentWeather.wind_speed) }}mph</p>
            </td>
            <td>
              <p><span class="bold">UV Index:</span> <br />{{ Math.ceil(currentWeather.uvi) }}</p>
            </td>
          </tr>
        </thead>
      </table>
    </div>
  </section>
</template>

<script>
export default {
  name: 'WeatherDash',
  props: ['currentWeather', 'isDaytime'], // Accepting isDaytime prop
  methods: {
    generateIconUrl(ico) {
      return `http://openweathermap.org/img/wn/${ico}@2x.png`;
    },
    generateDate(dt) {
      let dateTime = new Date(dt * 1000);
      let dateStr = dateTime.toDateString().split(' ');
      dateStr.splice(0, 1);
      dateStr.splice(dateStr.length - 1);
      return dateStr.join(' ');
    },
    generateDay(dt) {
      const days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let date = new Date(dt * 1000);
      let day = date.getDay();
      return days[day];
    },
  }
};
</script>

<style>
.today {
font-weight: 900
}
</style>