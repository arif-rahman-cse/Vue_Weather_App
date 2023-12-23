<template>
  <NavbarComp :title="title" />
  <WeatherContent v-if="!errored && !loading" :weatherDatas="weatherDatas" />
  <ErrorSection v-if="errored" :errormsg="errormsg" />
  <LoadingSection v-if="loading" :loadingmsg="loadingmsg" />
</template>

<script>
import axios from "axios";
import NavbarComp from "./components/NavbarComp.vue";
import WeatherContent from "./components/WeatherContent.vue";
import ErrorSection from "./components/ErrorSection.vue";
import LoadingSection from "./components/LoadingSection.vue";
import { reactive } from "vue";

export default {
  name: "App",
  components: { NavbarComp, WeatherContent, ErrorSection, LoadingSection },
  data() {
    return {
      loadingmsg: "Weather Data Loading...",
      errormsg:
        "We're sorry, free weathr api is not able to retrieve weather information at the moment, please try back later, we are currently using free version...",
      title: "Simple Weather Update System",
      weatherDatas: reactive([]),
      tempWeatherDatas: reactive([]),
      idx: 0,
      totalCity: 0,
      loading: true,
      errored: false,
      weatherApiKey: process.env.VUE_APP_API_KEY,
      weatherRootApi: process.env.VUE_APP_WEATHER_API_BASE_URL,
      cityRootApi: process.env.VUE_APP_CITY_API_BASE_URL,
      forecastDay: 6,
      forecastHour: 24,
      laguage: "ja",
    };
  },

  //This hook is called as soon as components mounted to the DOM,
  mounted() {
    // Call city api and then it will call weather api depending on number of city
    this.getCities();
    //If I need to update data without refreshing tha page.
    //setInterval(this.updatedWatherData, 10000);
  },

  methods: {
    //Get All the cities from backend and call weather api for weather data.
    getCities() {
      axios
        .get(`${this.cityRootApi}get-cities`)
        .then((response) => {
          this.totalCity = response.data.length;
          response.data.forEach((city) => {
            this.fetchWeatherData(city.city_name);
          });
        })
        .catch((error) => {
          console.log(error);
          this.errored = true;
        })
        .finally(() => {
          //this.loading = false;
        });
    },
    //Fetch weather data for individual cities and assign them to an array.
    fetchWeatherData(city) {
      axios
        .get(
          `${this.weatherRootApi}forecast.json?q=${city}&days=${this.forecastDay}&hour=${this.forecastHour}&lang=${this.laguage}&key=${this.weatherApiKey}`
        )
        .then((response) => {
          this.tempWeatherDatas.push(response.data);
          this.idx++;
          //Assign all data at a time so that reactive properties are not changed
          if (this.idx === this.totalCity) {
            this.weatherDatas = this.tempWeatherDatas;
            this.loading = false;
          }
        })
        .catch((error) => {
          console.log(error);
          this.errored = true;
        });
    },
    //If we need to update weather data after specific time interval call this function
    updatedWatherData() {
      this.idx = 0;
      this.tempWeatherDatas = []; // Clear the existing temp data before updating
      this.getCities();
    },
  },
  updated() {
    console.log("Data updated..");
    //Every time data update this life cylce hook will be called
  },
};
</script>

<style>
.weather_card {
  background: linear-gradient(to right, #d8d5eb, #008dd5);
  /*  0, 0, 0 is black (RGB), and 0.35 is the alpha (transparency) value. 
  0px: horizontal offset of the shadow.
  5px: vertical offset of the shadow
  15px: blur radius of the shadow. 
  */
  box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
  /* background: white;  */
  border-radius: 10px;
  background-color: #8ec5fc;
  background-image: linear-gradient(62deg, #8ec5fc 0%, #e0c3fc 100%);
}
body {
  background: #fff;
  margin: 0;
}
</style>
