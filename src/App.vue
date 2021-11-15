<template>
  <div className="background_img"></div>
  <div class="container">
    <div class="main">
      <div class="search">
        <input
          type="search"
          v-model="city"
          placeholder="&#128269; Search City"
        />
        <button @click="onclick()" class="btn">Search</button>
      </div>
      <!--   Search div Finished -->
      <template v-if="show">
        <div class="weatherIcon">
          <img :src="toggleIcon" alt="clear day" width="60px" />
          <span>{{ myImg.description }}</span>
        </div>

        <div class="temprature">
          <h3>{{ res.temp }}&#8451;<!-- temp here --></h3>

          <h2>
            <span class="feel"
              >City : {{ cityN }}<sup>`{{ countryN.country }}</sup></span
            >
          </h2>
          <p>
            <span>max : {{ res.temp_min }}&#8451; </span> || {{ " " }}
            <span>min : {{ res.temp_max }}&#8451; </span>
          </p>
          <p>
            <span class="feel"> Feel Like : {{ res.feels_like }}&#8451; </span
            >{{ " " }} ||
            <span class="feel">Humidity : {{ res.humidity }}</span>
          </p>
        </div>
      </template>
      <template v-else> city not found </template>
    </div>
  </div>
</template>

<script>
import Api from "./components/api/Api";
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      city: "faisalabad",
      toggleIcon: "",
      res: {},
      countryN: {},
      cityN: {},
      myImg: {},
      show: true,
      // Icons data
    };
  },
  methods: {
    async load() {
      try {
        const URL = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=675a0dbdd9e7aaf0ec4db2b587eb87aa`;

        this.city = "";
        const response = await axios.get(URL);

        // this.alldata = response;
        console.warn(response.status);

        this.res = response.data.main;
        this.countryN = response.data.sys;
        this.cityN = response.data.name;
        // console.log("Api Data ", this.res);

        if (response.status === 200) {
          this.myImg = response.data.weather[0];

          const icon = this.myImg.icon;
          // console.log(icon);
          this.show = true;

          switch (icon) {
            case "01d":
              this.toggleIcon = Api.dclear;
              break;
            case "02d":
              this.toggleIcon = Api.dfewClouds;
              break;
            case "03d":
              this.toggleIcon = Api.cloudy;
              break;
            case "04d":
              this.toggleIcon = Api.dbrokenClouds;
              break;
            case "09d":
              this.toggleIcon = Api.showerRain;
              break;
            case "10d":
              this.toggleIcon = Api.drain;
              break;
            case "11d":
              this.toggleIcon = Api.dthunderStorm;
              break;
            case "13d":
              this.toggleIcon = Api.dsnow;
              break;
            case "50d":
              this.toggleIcon = Api.dmist;
              break;
            case "01n":
              this.toggleIcon = Api.nclear;
              break;
            case "02n":
              this.toggleIcon = Api.nfewClouds;
              break;
            case "03n":
              this.toggleIcon = Api.cloudy;
              break;
            case "04n":
              this.toggleIcon = Api.nbrokenClouds;
              break;
            case "09n":
              this.toggleIcon = Api.showerRain;
              break;
            case "10n":
              this.toggleIcon = Api.nrain;
              break;
            case "11n":
              this.toggleIcon = Api.nthunderStorm;
              break;
            case "13n":
              this.toggleIcon = Api.nsnow;
              break;
            case "50n":
              this.toggleIcon = Api.nmist;
              break;
            default:
              break;
          }
        } else {
          console.log("i am from else");
          this.show = false;
        }

        // console.log(this.toggleIcon);
      } catch (err) {
        this.show = false;
        console.log(err.message);
      }
    },
    onclick() {
      this.load();
      // this.toggle();
    },
  },
  // components: {Api},
  mounted() {
    this.load();
  },
};

// console.log(this.Api); //image api integrate it
</script>

<style>
@import url("./style.css");
</style>
