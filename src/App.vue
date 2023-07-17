<script>
import axios from "axios";
import "./assets/App.css";

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null,
    };
  },

  computed: {
    cityName() {
      return this.city;
    },
    showTemp() {
      return "Temperature: " + this.info.main.temp + "	\xB0" + "C";
    },
    showFeelsLike() {
      return "Feels like: " + this.info.main.feels_like + "	\xB0" + "C";
    },
    showMinTemp() {
      return (
        "Minimum day temperature: " + this.info.main.temp_min + "	\xB0" + "C"
      );
    },
    showMaxTemp() {
      return (
        "Maximum day temperature: " + this.info.main.temp_max + "	\xB0" + "C"
      );
    },
    showMain() {
      return this.info.weather[0].description;
    },
    showImg() {
      if (
        this.info.weather[0].main === "Thunderstorm" ||
        this.info.weather[0].main === "Rain"
      ) {
        return "rain";
      } else if (this.info.weather[0].main === "Clouds") {
        return "clouds";
      } else if (this.info.weather[0].main === "Clear") {
        return "sunny";
      } else {
        return "main";
      }
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "More than one symbol required";
        return false;
      }
      this.error = "";

      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=3d9de74844d28377e81415151cbe6a66`
        )
        .then((res) => (this.info = res.data));
    },
  },
};
</script>

<template>
  <div class="sunny">
    <div class="wrapper">
      <h1 class="title">Weather App</h1>
      <p class="subtitle">
        Check the weather in {{ city == "" ? "your city" : cityName }}
      </p>
      <input type="text" v-model="city" placeholder="enter the city" />
      <button v-if="city != ''" @click="getWeather()">Get weather!</button>
      <button disabled v-else>Enter the name of the city</button>
      <p class="error">{{ error }}</p>

      <div class="result_box" v-if="info != null">
        <p class="result_text">{{ showTemp }}</p>
        <p class="result_text">{{ showFeelsLike }}</p>
        <p class="result_text">{{ showMinTemp }}</p>
        <p class="result_text">{{ showMaxTemp }}</p>
        <p class="result_text">{{ showMain }}</p>
        
      </div>
    </div>
  </div>
</template>
