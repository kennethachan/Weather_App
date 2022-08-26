<template>
  <section>
    <p class="heading bold">This Week</p>
    <div class="items">
      <div class="day" v-for="(data, index) in dailyWeather" :key="index">

      <table>
  <thead>
    <tr>
       <td> <p class="dayy">{{ generateDay(data.dt) }}</p><p>{{ generateDate(data.dt) }}</p></td>
      <td><img class="pic" :src="generateIcon(data.weather[0].icon)" /></td>
            <td><p class="high">High:{{ Math.ceil(data.temp.max) }}&#176;</p>
        <p class="low">Low:{{ Math.ceil(data.temp.min) }}&#176;</p></td>
      <td> <p><span class="bold">Humidity:</span> {{ data.humidity}}%</p></td>
      <td> <p><span class="bold">Precipitation:</span><br/>{{ Math.round(data.pop*100)}}%</p></td>
            <td><p><span class="bold">Wind Speed: </span>{{ Math.ceil(data.wind_speed)}}mph</p></td>
      <td><p><span class="bold">UV Index:</span> <br/>{{ Math.ceil(data.uvi)}}</p></td>
    </tr>
  </thead>
</table>
      </div>
    </div>
  </section>
</template>

<script>
  export default {
    name: 'WeeklyForecast',
    props: ['dailyWeather'],
    methods: {
      generateIcon(ico) {
        return `http://openweathermap.org/img/wn/${ico}@2x.png`
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
      generateDate(dt) {
        let dateTime = new Date(dt * 1000)
        let dateStr = dateTime.toDateString().split(' ')
        dateStr.splice(0, 1)
        dateStr.splice(dateStr.length - 1)
        return dateStr.join(' ')
      }
    }
  }
</script>

<style>
table {
  width:700px;
  background-color:#F0E6CE;
  border-collapse: collapse;
  border-radius: 20px;
      margin-left:auto;
    margin-right: auto;
    display: block;
    margin-bottom: 20px;
    box-shadow: rgba(0, 0, 0, 0.16) 0px 3px 6px, rgba(0, 0, 0, 0.23) 0px 3px 6px;
}

td {
  width: 100px;
  /* border: 1px solid black; */
  padding: 0.5rem;
  text-align: center;
}
  /* section {
    width: 100vw;
  } */
  
  .items {
     display: block;
         margin-left:auto;
    margin-right: auto;
  }
  .day {
    /* background-color: #F0E6CE; */
    width: 800px;
    margin-left:auto;
    margin-right: auto;
  }

  .dayy{
    font-weight: bold;
    color: #17539C;
  }

  .pic{
    margin-left:auto;
    margin-right: auto;
  }

  .high{
    color: #DB5E36;
    font-weight: bold;
  }

  .low{
    color: #4182AC;
     font-weight: bold;
  }

  .bold{
     font-weight: bold;
  }
</style>



