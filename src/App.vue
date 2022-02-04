<template>
  <div id="app"
  :class="{warm: warm, chilly: chilly, cold: cold}"
  >
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="enter city..."
        v-model="query"
        @keyup.enter="getWeather"
        >
      </div>
      <div class="weather-wrap"
      v-if="responseData"
      >
        <div class="location-box">
          <div class="location">{{responseData.name}}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{responseData.main.temp}}</div>
          <div class="weather">{{responseData.weather[0].main}}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  API_KEY: 'fe8ec3ef43fe35329c3a1537d3bb09e2',
  BASE_URL: 'https://api.openweathermap.org/data/2.5',
  name: 'App',
  data(){
    return {
      query: '',
      responseData: null
    }
  },
  methods: {
   async getWeather(){
     await axios.get(`${this.$options.BASE_URL}/weather?q=${this.query}&units=metric&APPID=${this.$options.API_KEY}`)
      .then(e => this.responseData = e.data);
      console.log(this.responseData)
    }
  },
  computed: {
    warm(){
      return this.responseData?.main.temp > 10;
    },
    chilly(){
      return this.responseData?.main.temp < 10 && this.responseData?.main.temp >= 0;
    },
    cold(){
      return this.responseData?.main.temp < 0;
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body{
  font-family: 'Times New Roman', Times, serif;
}

#app{
  background-size: cover;
  background-position: bottom;
  transition: 0.2s;
  min-height: 100vh;
}
.search-box{
width: 100%;
margin-bottom: 30px;
}
.search-box .search-bar{
  display: block;
  width: 30%;
  margin: auto;
  margin-top: 15px;
  padding: 15px;
  color: #131313;
  font-size: 15px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
}
.search-box .search-bar:focus{
  background-color: rgba(255, 255, 255, 0.85);
}
.location-box .location{
  color: rgb(255, 255, 255);
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.weather-box{
  text-align: center;
}
.weather-box .temp{
  padding: 10px 25px;
  color: white;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather{
 color: rgb(255, 255, 255);
 font-size: 24px;
 font-weight: 700;
 font-style: Italic;
 text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.warm{
  background-image: url('./assets/summer.jpg');
}
.chilly{
   background-image: url('./assets/fall.jpg');
}
.cold{
  background-image: url('./assets/winter.jpg');
}
</style>
