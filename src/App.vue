<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
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
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'" >
        <div class="location-box">
          <div class="location">
            {{ weather.name}},
            {{ weather.sys.country}}
          </div>
          <div class="date">
            {{ dateBuilder() }}
          </div>
        </div>
        <div class="weather-box">
          <div class="temp">
            <!-- 四捨五入 -->
            {{ Math.round(weather.main.temp) }}
          </div> 
          <div class="weather">
            {{ weather.weather[0].main }}
          </div> 
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      // OpenWeatherのAPIを使用
      api_key: '75f608587d64308ab3750ca2c42bdaa3',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter" ) {
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
      return `${day} ${date} ${month} ${year}`;
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

body {
  font-family: 'montserrat', sans-serif;
}

#app {
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpg');
}

main{
  /* vh: viewport height (1vh = 1%) */
  /* コンテンツ量が画面サイズより大きい場合、heightではなくmin-heightで指定するのがポイント */
  /* 表示デバイスによって高さが変わってしまうようなデザインでも簡単に対応させることができる */
  min-height: 100vh;
  padding: 25px;
  /* linear-gradient: 線形グラデーション rgba(赤の割合, 緑の割合, 青の割合,  透明度) */
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  /* 要素が横までいっぱいに広がり、縦に並んでいく */
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  /* ブラウザ別に設定されているデフォルトスタイルを無効 */
  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0 16px 0 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0 16px 0;
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  /* フォントの太さ指定 */
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  /* font-style: italic; */
  text-align: center; 
}

.weather-box {
 text-align: center;  
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  /* font-style: italic; */
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
