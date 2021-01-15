<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ?'warm' : ''">

    <main>
      <!-- Input -->
     <div class="search-box">
       <input 
       type="text" 
       placeholder="Search..." 
       class="search"
       v-model="query"
       @keypress="fetchWeather"
       >
     </div>
     
     <!-- Main Container for Weather Info -->
     <div class="weather-wrapper" v-if="typeof weather.main != 'undefined'">

       <!-- Location/Date Info  -->
       <div class="location">
        {{ weather.name }}, {{ weather.sys.country }}
       </div>

       <div class="date">
        {{ getCurrentDate() }}
       </div>

       <div class="temp">
        {{Math.round(weather.main.temp)}}°c
       </div>

       <div class="weather">
         {{ weather.weather[0].main }}
       </div>

     </div>
     
    </main>

  </div>
</template>

<script>

export default {
  name: 'App',

  data() {
  return {
  api_key: '86ffd031cbff5e1e45536771bd5abd57',
  start_url: 'https://api.openweathermap.org/data/2.5/',
  query: '',
  weather: {}
  }  
  },

  methods: {
  fetchWeather(e) {
  if(e.keyCode == 13) {
  fetch(`${this.start_url}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
  .then(res => res.json())
  .then(this.createObj)
  .catch(err => console.log(err));
  }
  },

  createObj(results) {
  this.weather = results;
  },

  getCurrentDate() {
  let d = new Date();  
  let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
  let months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
  
  let day = days[d.getDay()];
  let month = months[d.getMonth()];
  let date = d.getDate();
  let year = d.getFullYear();

  return `${day} ${date} ${month} ${year}`;

  }

  

  },

}
</script>

<style>
/* General Styling for all elems */
* {
margin: 0;
padding: 0;
box-sizing: border-box;
}

body {
font-family: 'montserrat', sans-serif;  
}

main {
min-height: 100vh;
padding: 25px;  
background: rgba(0, 0, 0, 0.5);
}

#app {
background-image: url('./assets/cold-bg.jpg');
background-repeat: no-repeat;
background-size: cover;
background-position: bottom;
}

/* Search Bar Container */
.search-box{
 width: 100%;
 margin-bottom: 30px;
 text-align: center;
}

/* Search Bar itself */
.search-box .search{
 width: 100%; 
 padding: 15px; 
 font-size: 20px;
 outline: none;
 border: none;
 border-radius: 0px 16px 0px 16px;
}

.search-box .search:focus{
border-radius: 16px 0px 16px 0px;
box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
background-color: rgba(255, 255, 255, 0.75);
}

.weather-wrapper{
text-align: center;  
}

.weather-wrapper .location{
color: #fff;  
font-size: 32px;
font-weight: 400;
text-shadow: 0px 3px 0px rgba(0, 0, 0, 0.25);
}

.weather-wrapper .date{
color: #fff;  
font-size: 20px;
font-style: italic;
}

.weather-wrapper .temp{
display: inline-block;  
color: #fff;  
font-size: 100px;
font-weight: 900;
text-shadow: 0px 6px 0px rgba(0, 0, 0, 0.25);
background: rgba(255, 255, 255, 0.25);
border-radius: 16px;
margin: 30px 0px;
padding: 10px 25px;
box-shadow:3px 6px rgba(0, 0, 0, 0.25) ;
}

.weather-wrapper .weather{
color: #fff;  
font-size: 40px;
font-weight: 700;
text-shadow: 0px 3px 0px rgba(0, 0, 0, 0.75);
font-style: italic;
}

#app.warm{
background-image: url('./assets/warm-bg.jpg');  
}


</style>
