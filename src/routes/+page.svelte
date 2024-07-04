<script lang="ts">
import { onMount } from 'svelte';

let time
let date
let lat = 60.1894
let lon = 24.5984
let currentWeather
let weatherPrediction

const ONE_HOUR = 60 * 60 * 1000; // 1 hour in milliseconds

async function getCurrentWeather() {
  let data = localStorage.getItem('currentWeather');
  let timestamp = localStorage.getItem('currentWeatherTimestamp');

  if (data && timestamp && (Date.now() - Number(timestamp)) < ONE_HOUR) {
    return JSON.parse(data);
  }


  console.warn('fetching new data');
  const url= `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${import.meta.env.VITE_OPENWEATHER_API_KEY}`;
  const response = await fetch(url);
  data = await response.json();

  localStorage.setItem('currentWeather', JSON.stringify(processCurrentWeather(data)));
  localStorage.setItem('currentWeatherTimestamp', Date.now().toString());

  return data;
}

async function getWeatherPrediction() {
  let data = localStorage.getItem('weatherPrediction');
  let timestamp = localStorage.getItem('weatherPredictionTimestamp');

  if (data && timestamp && (Date.now() - Number(timestamp)) < ONE_HOUR) {
    return JSON.parse(data);
  }

  console.warn('fetching new data');
  const url= `https://api.openweathermap.org/data/2.5/forecast?lat=${lat}&lon=${lon}&appid=${import.meta.env.VITE_OPENWEATHER_API_KEY}`;
  const response = await fetch(url);
  data = await response.json();
  console.log(data)

  localStorage.setItem('weatherPrediction', JSON.stringify(getNoonPrediction(data.list)));
  localStorage.setItem('weatherPredictionTimestamp', Date.now().toString());

  return data;
}

function processCurrentWeather(data) {
  return {
    temperature: toCelcius(data.main.temp),
    icon: data.weather[0].icon
  } 
}

function toCelcius(kelvin) {
  return parseFloat((kelvin - 273.15).toFixed(1));
}

function updateTime() {
  time = new Date().toLocaleTimeString('fi-FI', { hour: '2-digit', minute: '2-digit' });
  const [ weekday, dateStamp ] = new Date().toLocaleDateString('fi-FI', { weekday: 'long', year: 'numeric', month: 'numeric', day: 'numeric' }).split(" ");
  date = `${ weekday[0].toUpperCase() + weekday.slice(1, -2) } ${dateStamp}`
}

setInterval(() => {
  updateTime()
}, 5000);

onMount(async () => {
  updateTime()
   weatherPrediction = await getWeatherPrediction()
   currentWeather = await getCurrentWeather()
  console.log(weatherPrediction,'moi')
  console.log(currentWeather,'hei')
})

function getNoonPrediction(predictions) {
  const noonPrediction = predictions.find(prediction => {
    const date = new Date(prediction.dt_txt);
    return date.getHours() === 12;
  });

  return {
    temperature: toCelcius(noonPrediction.main.temp),
    icon: noonPrediction.weather[0].icon
  };
}
</script>


<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Itim&family=Lekton:ital,wght@0,400;0,700;1,400&family=Londrina+Shadow&display=swap" rel="stylesheet">

<div class="wrapper">
    <h3> {date} </h3>
    <h2 class="time">{time}</h2>

  <div class="weather">
  <div class="weather-item">
      <img src={currentWeather && `/weather-icons/${currentWeather?.icon}.png`} alt="weather icon"/>
  <p> {currentWeather?.temperature}</p> 
    </div>
<div class="weather-item">

      <img src={weatherPrediction && `/weather-icons/${weatherPrediction?.icon}.png`} alt="weather icon"/>
  <p> {weatherPrediction?.temperature}</p> 
    </div>    


  </div>
  <img src="/1.jpg"/>
</div>

<style>

* {
  box-sizing: border-box;
}

h1, h2, h3 {
  font-family: 'Itim', cursive;
  margin: 0;
}

 h2 {
  margin: 1rem 0;
  }

h3 {
  font-size: 1.2rem;
}

p {
  padding: 0 0.5rem;
  text-align: center;
}

.memo {
  color: var(--highlight);
  font-family: 'Lekton', sans-serif;
  font-weight: 900;
}


@keyframes full-horizontal-scroll {
  0% {
    transform: translateX(200%);
  }
  100% {
    transform: translateX(-200%);
  }
}
 
 body {
  margin: 0;
  padding: 0;
}

.time {
  font-size: 5rem;
}

.weather {
  margin-bottom: 2rem;
  display: flex;
  justify-content: center;
  gap: 0.5rem;
}

.weather-item {
  display: flex;
  align-items: center;
  font-family: 'Londrina Shadow', cursive;
  color: black;
  background-color: lightyellow;
  border-radius: 54px;
  font-size: 1.5rem;
  padding: 0.4rem;
  & p {
    margin: 0;
  }
  & img {
    width: 40px;
  }
}

.full-horizontal-scroll {
  white-space: nowrap;
  animation: full-horizontal-scroll 10s linear infinite;
}

.tomorrow {
  background-color: rgba(255, 255, 255, 0.8);
}

:root {
  --primary: cyan;
  --secondary: magenta;
  --highlight: yellow;
  background-image: url('/bg1.png');
  color: white;
  padding: 0;
  margin: 0;
  width: 100%;
  background-color: black;
     display: flex;
  align-items: center;
  height: 100vh;
}

.wrapper {
  display: flex;
  width: 100vw;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 2rem 0;

  border-top: 5px solid var(--primary);
  border-bottom: 5px solid var(--primary);
  }


img {
  width: 100%;
  max-width: 500px;
  height: auto;
}

</style>
