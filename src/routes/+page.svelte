<script lang="ts">
let time
let date

function updateTime() {
  time = new Date().toLocaleTimeString('fi-FI', { hour: '2-digit', minute: '2-digit' });
  const [ weekday, dateStamp ] = new Date().toLocaleDateString('fi-FI', { weekday: 'long', year: 'numeric', month: 'numeric', day: 'numeric' }).split(" ");
  date = `${ weekday[0].toUpperCase() + weekday.slice(1, -2) } ${dateStamp}`
}

setInterval(() => {
  updateTime()
}, 5000);

updateTime()
</script>


<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Itim&family=Lekton:ital,wght@0,400;0,700;1,400&family=Londrina+Shadow&display=swap" rel="stylesheet">

<div class="wrapper">
    <h3> {date} </h3>
    <h2 class="time">{time}</h2>

  <div class="weather">
  <div class="weather-item">
  <p> 🌧️ 10.5</p> 
    </div>

  <div class="weather-item tomorrow">
<p> 🌞 20.5 </p>
    </div>
    </div>
  <p> Lovely evening to you! </p>
  <p class="memo"> TOMORROR RAY TO DAYCARE ON TIME!!! <p>
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
  font-size: 7rem;
}

.weather {
  display: flex;
  justify-content: center;
  gap: 1rem;
}

.weather-item {
  font-family: 'Londrina Shadow', cursive;
  color: black;
  background-color: white;
  border-radius: 54px;
  font-size: 1.5rem;
  padding: 0.5rem;
  & p {
    margin: 0;
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
  height: auto;
}

</style>
