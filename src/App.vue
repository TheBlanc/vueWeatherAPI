<template>
  <div id="app" :class="computedBackgroundImageClass">
    <main :class="computedGradientClass">
      <div class="search-box">
        <input
          type="text"
          class="input search-bar"
          placeholder="Search a city..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="container content" v-if="typeof weather.main == 'undefined'">
        <h1 class="has-text-white mt-6 default-text">Check the Weather!</h1>
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather-icon" :class="computedWeatherIconClass"></div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      api_key: 'ed292d226d771dbe0ea0aaf99ca95f2f',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${month} ${date}, ${year}`;
    }
  },
  computed: {
    computedBackgroundImageClass() {
      var className = "";
      if (typeof this.weather.main != 'undefined' && this.weather.main.temp > 1 && this.weather.main.temp <= 20) {
        className = "warm";
      } else if (typeof this.weather.main != 'undefined' && this.weather.main.temp > 20) {
        className = "hot";
      } else if (typeof this.weather.main != 'undefined' && this.weather.main.temp <= 1) {
        className = "cold";
      } else {
        className = "";
      }
       return className;
    },
    computedGradientClass() {
      var className = "";
      if (typeof this.weather.main != 'undefined' && this.weather.weather[0].main == 'Clear') {
        className = "clear";
      } else if (typeof this.weather.main != 'undefined' && this.weather.weather[0].main == 'Rain') {
        className = "rain";
      } else if (typeof this.weather.main != 'undefined' && this.weather.weather[0].main == 'Thunderstorm') {
        className = "thunder";
      } else if (typeof this.weather.main == 'undefined') {
        className = "";
      } else {
        className = "cloudy";
      }
       return className;
    },
    computedWeatherIconClass() {
      var className = "";
      if (typeof this.weather.main != 'undefined' && this.weather.weather[0].description == 'few clouds: 11-25%') {
        className = "partial-sun";
      } else if (typeof this.weather.main != 'undefined' && this.weather.weather[0].main == 'Clear' ) {
        className = "sun";
      } else if (typeof this.weather.main != 'undefined' && this.weather.weather[0].main == 'Rain') {
        className = "rain";
      } else if (typeof this.weather.main != 'undefined' && this.weather.weather[0].main == 'Snow') {
        className = "snow";
      } else if (typeof this.weather.main != 'undefined' && this.weather.weather[0].main == 'Thunderstorm') {
        className = "thunder";
      } else if (typeof this.weather.main == 'undefined') {
        className = "";
      } else {
        className = "cloudy";
      }
       return className;
    }


  }
}
</script>


<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@900&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: monospace;
  background-color: #f4f4f4;
}

h1 {

  text-shadow: 1px 1px 6px rgb(0 0 0 / 71%);
}

#app {
  background-image: url('./assets/default-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
  max-width: 1000px;
  margin: auto;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpg');
}
#app.hot {
  background-image: url('./assets/hot-bg.jpg');
}
#app.cold {
  background-image: url('./assets/cold-bg.jpg');
}


main {
  text-align: center;
  min-height: 100vh;
  padding: 25px;
}
main.clear {
  background-image: linear-gradient(to bottom, #7979792b, #0000003b);
  transition: 0.4s;
}
main.cloudy {
  background-image: linear-gradient(to bottom, rgb(0 0 0 / 11%), rgb(0 0 0 / 48%));
}
main.rain {
  background-image: linear-gradient(to bottom, rgb(0 0 0 / 32%), rgb(0 0 0 / 59%));
}
main.thunder {
  background-image: linear-gradient(to bottom, rgb(0 0 0 / 51%), rgb(0 0 0 / 62%));
}

.default-text {
  font-weight: bold;
  letter-spacing: 1.5px;
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-bar {
  width: 100%;
  max-width: 600px;
  margin: auto;
  color: #404040;
  font-size: 1.3rem;
  background-color: rgba(255, 255, 255, 0.80);
  border-radius: 16px;
  transition: 0.4s;
}
.search-bar:focus {
  border: none;
  box-shadow: none;
  background-color: rgba(255, 255, 255, 1);
}
.location-box .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: bold;
  font-style: italic;
  text-align: center;
}
.weather-box {
  font-family: 'Montserrat', sans-serif;
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 4px 5px 1px rgb(0 0 0 / 29%);
  border-radius: 16px;
  line-height: 5rem;
  margin-top: 1.5rem;
}
.weather-box .weather {
  font-family: monospace;
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  line-height: 50px;
  text-shadow: 2px 4px rgba(0, 0, 0, 0.25);
}

.weather-icon {
  margin: auto;
  margin-top: 10px;
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
  width: 90px;
  height: 90px;
}

.weather-icon.partial-sun {
  background-image: url('./assets/partial-sun.png');
}
.weather-icon.sun {
  background-image: url('./assets/sun.png');
}
.weather-icon.cloudy {
  background-image: url('./assets/cloudy.png');
}
.weather-icon.rain {
  background-image: url('./assets/rain.png');
}
.weather-icon.snow {
  background-image: url('./assets/snow.png');
}
.weather-icon.thunder {
  background-image: url('./assets/thunder.png');
}
.weather-icon.wind {
  background-image: url('./assets/wind.png');
}

</style>
