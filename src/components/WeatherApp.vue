<template>

  <div class="search">
    <input ref="cit" class="search__input" type="text" placeholder="Введите город"  @keyup.enter="getWeather(this.$refs.cit.value)">
    <button class="search__btn" @click="getWeather(this.$refs.cit.value)">
      <i class="fa-solid fa-magnifying-glass"></i>
    </button>
  </div>

  <div class="weather">
    <h3 class="weather__location">{{country}} {{city}}</h3>
    <p class="weather__time">{{time}}</p>
    <img class="weather__icon" :src="`https://openweathermap.org/img/wn/${icon}@2x.png`"/>
    <p class="weather__temp">{{temp}} &deg;C</p>
  </div>

  <div class="weatherData">
    <div v-for="item in items" :key="item" class="weatherData__item">
      <p>{{item.dt_txt.split(" ")[1].slice(0, 5)}}</p>
      <img :src="`https://openweathermap.org/img/wn/${item.weather[0].icon}@2x.png`"/>
      <p>{{Math.round(item.main.temp - 273.15)}} &deg;C</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: "WeatherData",
  created() {
    axios.get(`https://get.geojs.io/v1/ip/geo.json`)
        .then(res => {
          return res.data;
        })
        .then(data => {
          this.city = data.city;
          this.getWeather(this.city);
        })
  },
  data() {
    return {
      city: '',
      country: '',
      temp: '',
      icon: '',
      time: '',
      items: [],
    }
  },
  methods: {

    getWeather: function(city) {
      axios.get(`https://api.openweathermap.org/data/2.5/forecast?q=${city}&appid=0d73a8fc0d8b65591a7df95089fe171c`)
          .then(res => {
            return res.data;
          })
          .then(data => {
            this.city = city;
            this.items = data.list.splice(1, 8);
            this.temp =  Math.round(data.list[0].main.temp - 273.15);
            this.time = data.list[0].dt_txt.split(" ")[1].slice(0, 5)
            this.icon = data.list[0].weather[0].icon;
            this.country = data.city.country;
          })

    }
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Source+Code+Pro&display=swap');

.weather {
  background-color: rgba(255, 255, 255, .5);
  width: 500px;
  margin: 0 auto;
  border-radius: 10px;
  border: 3px solid #000;
  margin-bottom: 15px;
  margin-top: 15px;
  font-family: 'Source Code Pro', monospace;
  animation-name: item;
  animation-duration: 0.5s;
}

.weather__location {
  font-size: 40px;
}

.weather__temp, .weather__time {
  font-size: 30px;
}

.weatherData {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
}

.weatherData__item {
  border: 3px solid #000;
  padding: 15px;
  background-color: rgba(255, 255, 255, .5);
  font-family: 'Source Code Pro', monospace;
  font-size: 30px;
  width: 180px;
  border-radius: 10px;
  margin-bottom: 10px;
  animation-name: item;
  animation-duration: 0.3s;
}

.weatherData__item:hover {
  box-shadow: 10px 5px 10px rgba(0, 0, 0, 0.2);
}

@keyframes item {
  0%   { opacity: 0; }
  100% { opacity: 1; }
}

.search__input {
  width: 300px;
  padding: 10px 20px;
  border-radius: 5px;
  border: 1px solid #000000;
  background: rgba(255, 255, 255, 0.8);
  border-top-right-radius: 0;
  border-bottom-right-radius: 0;
}

.search__btn {
  width: 50px;
  height: 38px;
  border-top-left-radius: 0;
  border-bottom-left-radius: 0;
}

</style>