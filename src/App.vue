<template>
  <div id="app" :class="typeof weather.main !='undefined' && weather.main.temp >16 ? 'warm' : '' ">
    <main>
      <h1>Weather</h1>
      <div class="search-box">
        <input 
          type="text" 
          placeholder="Search the city ..." 
          class="search-bar" 
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main !='undefined' ">
        <div class="location-box">
          <div class="location"> {{weather.name}}, {{weather.sys.country}} </div>
          <div class="date">{{dateBuilder()}}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}°</div>
          <div class="weather">{{weather.weather[0].main}}</div>
        </div>
      </div>
      
    </main>
    
  </div>
</template>

<script>


export default {
  name: 'App',
  data(){
    return {
      api_key : '0e6051b8f8e9cfbc1c46bbd93bd0156c',
      url_base: "http://api.openweathermap.org/data/2.5/" ,
      query: '',
      weather: {}
    }
  },
  methods:{
      fetchWeather(e){
        if(e.key =="Enter"){
          fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${ this.api_key}`)
            .then(res=>{
              return res.json();
            }).then(this.setResults);
        }
      },
      setResults(results){
        this.weather =results; 
      },
      dateBuilder(){
        let d= new Date();
        let months=["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
        let days = [ "Monday", "Tuesday", "Wednesday", "Thursday", "Friday" ];
        let day=days[d.getDay()];
        let date = d.getDate();
        let month = months[d.getMonth()];
        let year = d.getFullYear();
        
        return `${day} ${date} ${month} ${year}`
      }
  }
}
</script>

<style>
  *{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  h1{
    text-align: center;
    margin: 20px ;
    font-weight: 500;
    text-shadow:  1px 3px rgba(0,0,0,0.25);
    font-size: 40px;
    color: #fff;
  }
  body{
    font-family: 'montserrat' ,sans-serif, georgia;
  }

  #app{
    background-image: url('./assets/cold.jpg');
    background-size: cover;
    background-position: bottom ;
    transition: 0.4s;
  }

  #app.warm{
    background-image: url('./assets/hot.jpg');
  }
  main{
    min-height: 700px;
    padding:45px;
    
    background-image: linear-gradient(to bottom, rgba(0,0,0,0.25),rgba(0,0,0,0.6));
  }

  .search-box{
    width: 100%;
    margin-bottom: 30px;
  }

  .search-box .search-bar{
    display: block;
    width:100%;
    padding:15px;
    
    color: #313131;
    font-size: 20px;

    appearance: none;
    border: none;
    outline: none;
    background: none;
    box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
    background-color: rgba(255,255,255,0.25);
    border-radius: 0px 15px 0px 15px;
    transition:0.4s;

  }
  .search-box .search-bar:focus{
    box-shadow: 0px 0px 30px rgba(0,0,0,0.25);
    background-color: rgba(255,255,255,0.75);
  }

  .location-box .location{
    color: #fff;
    font-size: 30px;
    font-weight: 500;
    text-align: center;
    text-shadow:  1px 3px rgba(0,0,0,0.25);
  }
  .location-box .date{
    color: #fff;
    font-size: 20px;
    font-weight: 300;
    font-style: initial;
    text-align: center;
    text-shadow:  1px 3px rgba(0,0,0,0.25);
  }

  .weather-box{
    text-align: center;
  }

  .weather-box .temp{
    display: block;
    padding: 10px 15px;
    color: #fff;
    font-size: 80px;
    width: 70%;
    
    font-weight: 700;
    text-shadow: 3px 6px rgba(0,0,0,0.25);
    background-color: rgba(255,255,255,0.25);
    border-radius: 18px;
    margin: 30px 15% ;
    justify-self: center;
    box-shadow: 3px 6px rgba(0,0,0,0.25);
  }
  .weather-box .weather{
    color:#fff;
    font-size: 45px;
    font-weight: 600;
    font-style: oblique;
    text-shadow: 3px 6px rgba(0,0,0,0.25);

  }
</style>
