<template>
  <div class="container">
    <div class="header">
      <div class="input-container">
        <input type="text" v-model="city">
        <button @click="fetchWeatherData(city)">
          Search
        </button>

      </div>
    </div>

    <div class="weather-content" v-if="weather.name">

      <div class="city">
        <h2>City</h2>
        <p>City: {{ weather.name }}</p>
        <p>Country code: {{ weather.countryCode }}</p>
      </div>
      <div class="info-container">

        <div class="box">
          <h2>Weather info</h2>
          <p>Weather: {{ weather.weatherType }}</p>
          <p>Description: {{ weather.weatherDescription }}</p>
          <img :src="`http://openweathermap.org/img/wn/${weather.icon}.png`" alt="Weather Icon">

        </div>
        <div class="box">
          <h2>Main</h2>
          <p>Temperature: {{ weather.temperature }}°C</p>
          <div v-if="weather.sunrise && weather.sunrise">
            <p>Min temp: {{ weather.temp_min }}°C</p>
            <p>Max temp: {{ weather.temp_max }}°C</p>
          </div>
          <p>Pressure: {{ weather.pressure }}</p>
          <p>Humidity: {{ weather.humidity }}</p>
        </div>


        <div class="box">
          <h2>Wind</h2>
          <p>Wind speed: {{ weather.windSpeed }}</p>
          <p>Wind deg: {{ weather.windSpeedDeg }}</p>
        </div>
        <div class="box">
          <h2>Sunrise / sunset</h2>
          <p>Sunrise: {{ formatTime(weather.sunrise) }} </p>
          <p>Sunset: {{ formatTime(weather.sunset) }} </p>
        </div>
      </div>
    </div>
    <div v-else style="flex: 3; width: 100%; justify-content: center; align-items: center;">

    </div>

  </div>
</template>


<script>
import { OhVueIcon } from "oh-vue-icons";


export default {
  data() {
    return {
      city: "",
      weather: {}
    }
  },
  components: {
    'v-icon': OhVueIcon
  },

  methods: {
    fetchWeatherData(city) {
      fetch(`https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=7af7c6755a2ba6d6003ad0af015a23c9&units=metric`)
        .then(response => {
          if (!response.ok) {
            throw new Error("Data not found!")
          }
          return response.json()
        })
        .then(data => {
          this.weather = {
            // Weather
            weatherType: data.weather[0].main,
            weatherDescription: data.weather[0].description,
            icon: data.weather[0].icon,
            // Main
            temperature: data.main.temp,
            pressure: data.main.pressure,
            humidity: data.main.humidity,
            minTemp: data.main.temp_min,
            maxTemp: data.main.temp_max,
            // Wind
            windSpeed: data.wind.speed,
            windSpeedDeg: data.wind.deg,
            // City
            name: data.name,
            countryCode: data.sys.country,
            // Sunrise / Sunset
            sunrise: data.sys.sunrise,
            sunset: data.sys.sunset
          }
          console.log(this.weather)
        })
        .catch(error => {
          console.log(error)
          alert("Failed to get data")
        })
    },
    formatTime(timestamp) {
      const date = new Date(timestamp * 1000)
      return date.toLocaleTimeString()
    }
  },

}
</script>


<style>
.container {
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
}

.header {
  background: url("../assets/sky.png");
  background-size: cover;
  background-repeat: no-repeat;
  display: flex;
  flex: 1;
  justify-content: center;
  align-items: center;
}

.input-container {
  display: flex;
  gap: 10px;
}

input {
  border: 2px solid rgb(46, 112, 226);
  padding: 5px 10px;
  font-size: 1.1rem;
  border-radius: 20px;
  color: rgb(46, 112, 226);
  font-weight: 500;
}

button {
  border: none;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 20px;
  color: white;
  background-color: rgb(46, 112, 226);
}

input,
button {
  outline: none;
}

.weather-content {
  border: 1px solid blue;
  background-color: rgb(245, 245, 245);
  flex: 3;
}
.info-container {
  padding: 20px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px;

}

.box {
  padding: 20px;
  background-color: orange;
  border-radius: 20px;
}
</style>