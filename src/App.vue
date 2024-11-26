<script>
import axios from 'axios'

export default {
  data() {
    return {
      city: '',
      error: '',
      info: null,
    }
  },
  computed: {
    citiName() {
      return '*' + this.city + '*'
    },
    showTemperature() {
      return 'Temperature: ' + this.info.main.temp
    },
    showFeelsLike() {
      return 'Feels Like: ' + this.info.main.feels_like
    },
    showMinTemperature() {
      return 'Min Temperature: ' + this.info.main.temp_min
    },
    showMaxTemperature() {
      return 'Max Temperature: ' + this.info.main.temp_max
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 3) {
        this.error = 'Need a name with more than one character'
        return false
      }
      this.error = ''

      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=8b66ae9e93726f93876fc0653d84201b`,
        )
        .then((res) => (this.info = res.data))
        .catch((err) => {
          this.error = 'City not found!'
          this.info = null
          console.log(err)
        })
    },
  },
}
</script>
<template>
  <div class="wrapper">
    <h1>Weather App</h1>
    <p>Find out the weather in {{ city == '' ? 'your city' : citiName }}</p>
    <input type="text" v-model="city" placeholder="Enter your city" />
    <button v-if="city != ''" @click="getWeather()">Get Weather</button>
    <button disabled v-else>Enter city name</button>
    <p class="error">{{ error }}</p>
    <div v-if="info != null">
      <p>{{ showTemperature }}</p>
      <p>{{ showFeelsLike }}</p>
      <p>{{ showMinTemperature }}</p>
      <p>{{ showMaxTemperature }}</p>
    </div>
  </div>
</template>
<style scoped>
.error {
  color: red;
}

.wrapper {
  width: 100%;
  max-width: 900px;
  height: auto;
  border-radius: 20px;
  padding: 20px;
  background: #87cfeb55;
  text-align: center;
  margin: 0 auto;
}

.wrapper h1 {
  color: rgb(38, 3, 70);
  font-size: 36px;
  margin-top: 20px;
  margin-bottom: 20px;
  text-transform: uppercase;
  letter-spacing: 2px;
  font-weight: bold;
  font-family: Arial, Helvetica, sans-serif;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
  animation: rainbow 2s infinite;
  animation-timing-function: linear;
}

.wrapper p {
  color: rgb(38, 3, 70);
  font-size: 24px;
  font-weight: bold;
  font-family: Arial, Helvetica, sans-serif;
  margin-bottom: 20px;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
}

.wrapper input {
  width: 80%;
  max-width: 300px;
  height: 40px;
  font-size: 15px;
  border-radius: 5px;
  padding: 5px 8px;
  margin-bottom: 20px;
  border: none;
  outline: none;
  border-bottom: 2px solid rgba(0, 0, 0, 0) transparent;
  background: #ffffff20;
  box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
}

.wrapper input:focus {
  border-bottom: 2px solid rgb(38, 3, 70);
  transition: border-color 0.2s ease;
}

.wrapper button {
  width: 100px;
  height: 40px;
  margin-left: 10px;
  border-radius: 5px;
  background: rgb(38, 3, 70);
  color: #ffffff;
  border: none;
  outline: none;
  cursor: pointer;
  box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
  transition: transform 0.2s ease;
}

.wrapper button:hover {
  transform: scale(1.03);
}

.wrapper button:disabled {
  background: #00e1ff93;
  cursor: not-allowed;
}

@media (max-width: 600px) {
  .wrapper {
    width: 100%;
    padding: 15px;
  }

  .wrapper h1 {
    font-size: 28px;
  }

  .wrapper p {
    font-size: 18px;
  }

  .wrapper input {
    width: 90%;
    max-width: 100%;
    font-size: 14px;
  }

  .wrapper button {
    width: 80%;
    font-size: 16px;
    margin-left: 0;
    margin-top: 10px;
  }
}
</style>
