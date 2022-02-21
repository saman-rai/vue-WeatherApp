<template>
  <div id="app" :class="weatherBg">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress.enter="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if='typeof weather.main != "undefined"'>
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>


export default {
  name: 'app',
  data (){
    return {
      api_key: "5e6c89a73932defa469384ed59140068",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    }
  },
  methods:{
    fetchWeather(){
      fetch(`${ this.url_base }weather?q=${ this.query }&units=metric&APPID=${ this.api_key }`)
        .then( res=> {
          return res.json()
        }).then(this.setResults);
    },
    setResults(results){
      this.weather = results;
    },
    dateBuilder(){
      let d = new Date();
      let days = ["Sunday", "Monday", "Tuesday", "Thursday", "Friday", "Saturday"];
      let months = ["January", "February", "March", "April", "May", "June", "July", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let day = days[d.getDay()];
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      let date = d.getDate();

      return  `${day} ${date} ${month} ${year}`;
    },
  },
  computed: {
    weatherBg(){
      if(typeof this.weather.main === 'undefined' ) return "none"
      
      else if(this.weather.main.temp >= 17 ) return "warm";
      else return "none"
    }
  },
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body{
  font-family: 'montserrat', sans-serif;
}
#app{
  background-image: url('./assets/cold-bg.jpg');
  background-position: bottom;
  background-size: cover;
  transition: .4s;
}
#app.warm{
  background-image: url('./assets/warm-bg.jpg');
}
main{
  min-height: 100vh;
  width: 100%;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,.25), rgba(0,0,0,.5));
}
.search-box{
  margin-bottom: 30px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.search-box .search-bar{
  display: block;
  padding: 15px;
  width: 100%;
  

  color: #313131;
  font-size: 20px;

  appearance: none;
  background: none;
  border: none;
  outline: none;

  box-shadow: 0 0 8px rgba(0,0,0,.25);
  background-color: rgba(255, 255, 255, .5);
  border-radius: 0px 16px 0px 16px;
  transition: .4s;
}

.search-bar{
  max-width: 750px;
}
.search-box .search-bar:focus{
  box-shadow: 0 0 16px rgba(0,0,0,.25);
  background-color: rgba(255, 255, 255, .75);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location{
  color: #fff;
  text-align: center;
  font-size: 32px;
  font-weight: 500;
  text-shadow: 1px 3px rgba(0,0,0,.25);
}
.location-box .date{
  color: #fff;
  text-align: center;
  font-size: 18px;
  font-weight: 300;
  font-style: italic;
}

.weather-box{
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.weather-box .temp{
  color: #fff;
  font-size: 80px;
  padding: 10px 25px;
  font-weight: 900;
  
  text-shadow: 3px 6px rgba(0,0,0,.25);
  box-shadow: 3px 6px rgba(0,0,0,.25);
  background-color: rgba(255, 255, 255, .25);
  margin: 30px 0;
  border-radius: 16px;

  width:100%;
  max-width: 750px;
}
.weather-box .weather{
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,.25);
}
</style>
