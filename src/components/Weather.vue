<template>
  <div class="container">
    <div class="header">
      <div class="input-container">
        <input type="text" v-model="city" placeholder="Oslo...">
        <button @click="fetchWeatherData(city)">
          Search
        </button>

      </div>
    </div>

    <div class="card" v-if="weather.name">
      <div class="card_header">
        <div class="card_header-city">{{ weather.name }},</div>
        <div class="card_header-country">{{ weather.countryCode }}</div>
        <img :src="`http://openweathermap.org/img/wn/10d.png`" alt="Weather Icon">
      </div>
      <div class="content">

        <div class="main">

          <div class="description">
            <p>{{ weather.weatherType }}</p>
            <p>{{ weather.weatherDescription }}</p>
          </div>
          <div class="description">
            <p>Sunrise</p>
            <p>{{ formatTime(weather.sunrise) }}</p>
          </div>
          <div class="description">
            <p>Sunset</p>
            <p>{{ formatTime(weather.sunset) }}</p>
          </div>

          <div class="temperature-container">
            <div class="current-temp">
              <p>{{ weather.temperature }}°C</p>
            </div>
          </div>

          <div class="unit-container">
            <div class="unit">
              <p>Pressure</p>
              <p>{{ weather.pressure }}</p>
            </div>
            <div class="unit">
              <p>Humidity</p>
              <p>{{ weather.humidity }}%</p>
            </div>
            <div class="unit">
              <p>Wind speed</p>
              <p>{{ weather.windSpeed }} m/s</p>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div v-else style="flex: 1; display: flex; justify-content: center;
    align-items: center;">
      <P style="font-size: 2rem; color: white ;">Enter a city....</P>
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
  background: linear-gradient(-35deg, rgb(0, 179, 255), rgb(45, 136, 241));
}

.header {
  background-size: cover;
  background-repeat: no-repeat;
  display: flex;
  align-items: center;
  height: 15vh;
  justify-content: center;
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
  color: rgb(57, 57, 57);
  font-weight: 500;
}

button {
  font-weight: 500;
  font-size: 1.1rem;
  border: none;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 20px;
  color: white;
  opacity: .5;
  background: linear-gradient(45deg, rgb(2, 120, 255), rgb(3, 121, 255));
  transition: all ease 250ms;
}

button:focus,
:hover {
  opacity: 1;
}

input,
button {
  outline: none;
}

.card {
  margin: 100px auto 0 auto;
  background-color: rgba(0, 0, 0, 0.5);
  border-radius: 15px;
  padding: 20px;
  height: 350px;
  width: 300px;
  box-shadow: 5px 5px 8px rgba(0, 0, 0, 0.2);
  color: white;
}

.card_header {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  font-size: 1.8rem;
  margin: 10px 0px;
}

.card_header-country {
  margin-left: 5px;
  color: rgb(70, 181, 255);
}

.description {
  display: flex;
  justify-content: space-between;
  border-bottom: 1px solid rgb(202, 202, 202);
}

.description p {
  margin: 0px;
  padding: 10px;
}

.temperature-container {
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 30px 0px;
}

.current-temp {
  font-size: 3rem;
}

.main {
  display: flex;
  flex-direction: column;
  height: 230px;
  justify-content: space-between;
}

.unit-container {
  display: flex;
  justify-content: space-between;
  margin-top: auto;
}

.unit {
  display: flex;
  gap: 2px;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

p {
  padding: 0%;
  margin: 0%;
}

img {
  width: 65px;
  aspect-ratio: 1;
}

.info-container {
  padding: 20px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px;

}</style>