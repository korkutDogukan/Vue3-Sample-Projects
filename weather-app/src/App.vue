<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''
    "
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>
<script setup>
import { ref, computed } from "vue";
const api_key = ref("b258f6e389533d687e4b5f7b7cf7f394");
const url_base = ref("https://api.openweathermap.org/data/2.5/");
const query = ref("");
const weather = ref({});

const fetchWeather = (e) => {
  console.log(e.key);
  if (e.key == "Enter") {
    fetch(
      `${url_base.value}weather?q=${query.value}&units=metric&APPID=${api_key.value}`
    )
      .then((res) => {
        return res.json();
      })
      .then(setResults);
  }
};

const setResults = (results) => {
  weather.value = results;
  console.log(weather.value);
};

const dateBuilder = computed(() => {
  return new Date().toDateString();
});
</script>
