<template>
  <div id="weather">
    <img src="../assets/refresh-button.png" alt="새로고침" v-on:click="get_weather" id="refresh"><br>
    <div v-if="sky=='SKY_A01'" class="weather_icon sun"><span>{{skyname}}</span></div>
    <div v-if="sky=='SKY_A02'" class="weather_icon cloud"><span>{{skyname}}</span></div>
    <div v-if="sky=='SKY_A03'||sky=='SKY_A11'" class="weather_icon cloud_many"><span>{{skyname}}</span></div>
    <div v-if="sky=='SKY_A04'||sky=='SKY_A08'||sky=='SKY_A12'" class="weather_icon rain"><span>{{skyname}}</span></div>
    <div v-if="sky=='SKY_A05'||sky=='SKY_A09'||sky=='SKY_A13'" class="weather_icon snow"><span>{{skyname}}</span></div>
    <div v-if="sky=='SKY_A06'||sky=='SKY_A10'||sky=='SKY_A14'" class="weather_icon rain-snow"><span>{{skyname}}</span></div>
    <div v-if="sky=='SKY_A07'" class="weather_icon fog"><span>{{skyname}}</span></div>
    <div id="station">
      {{station_name}}
    </div>
    <div id="weather_temperature">
      <div >
        <span id="tc">현재 기온 | {{tc}}℃</span>
      </div>
      <div>
        <span class="tmax">최고 기온 | {{tmax}}℃</span>&nbsp;
        <span class="tmin">최저 기온 | {{tmin}}℃</span>
      </div>
      <div id="humidity_dust">
        <span id="humidity">습도  |  {{humidity}}%</span>&nbsp;
        <span id="dust">미세 먼지 | {{dust_value}}㎍/㎥</span>
      </div>
    </div>
    <div id="tomorrow">
      <span id="tomorrow_title">내일은...</span>
      <div v-if="tomorrow_sky_code=='SKY_M01'" class="tomorrow_weather_icon sun"></div>
      <div v-if="tomorrow_sky_code=='SKY_M02'" class="tomorrow_weather_icon cloud"></div>
      <div v-if="tomorrow_sky_code=='SKY_M03'" class="tomorrow_weather_icon cloud_many"></div>
      <div v-if="tomorrow_sky_code=='SKY_M04'" class="tomorrow_weather_icon frog"></div>
      <div v-if="tomorrow_sky_code=='SKY_M05'" class="tomorrow_weather_icon rain"></div>
      <div v-if="tomorrow_sky_code=='SKY_M06'" class="tomorrow_weather_icon snow"></div>
      <div v-if="tomorrow_sky_code=='SKY_M07'" class="tomorrow_weather_icon rain-snow"></div>
    </div>
    <div id="tommorrow_temperature"><span class="tmax">최고 온도 | {{tomorrow_tmax}}℃</span> <span class="tmin">최저 온도 | {{tomorrow_tmin}}℃</span></div>
    <div id="link-box">
    <a id="link" v-bind:href="'https://search.naver.com/search.naver?query='+station_name+'+날씨'">더보기</a>
    </div>
  </div>
  
</template>
<script>
function setCookie(cname, cvalue, exdays) {
    var d = new Date();
    d.setTime(d.getTime() + (exdays * 24 * 60 * 60 * 1000));
    var expires = "expires=" + d.toUTCString();
    document.cookie = cname + "=" + cvalue + ";" + expires + ";path=/";
}

function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for (var i = 0; i < ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}
var lat, lon;
if(navigator.geolocation){
    navigator.geolocation.getCurrentPosition(function(position){
        lat = position.coords.latitude;
        lon = position.coords.longitude;
        setCookie('lat',lat,1);
        setCookie('lon',lon,1);
        }, function(error) {
          console.error(error);
        }, {enableHighAccuracy : false,
            maximumAge:0,
            timeout: Infinity
        });
      }
      else{
        alert('GPS가 지원되지 않습니다. 지역선택은 추후 지원할 예정입니다.');
      }
  var getXY = function(){
  var param = 'lat='+getCookie('lat')+'&lon='+getCookie('lon')+'&version=1';
  return param;
  }
export default {
  data(){
      name: 'Get-Weather'
      return{
        station_name : '알 수 없습니다!',
        msg: '날씨를 불러와 주세요',
        sky : 'error',
        dust_value: 'error', 
        dust_grade: 'error', 
        skyname: 'error', 
        tc: 'error', 
        tmax: 'error', 
        tmin: 'error', 
        humidity: 'error',  
        tomorrow_sky_code: 'error',
        tomorrow_tmax: 'error',
        tomorrow_tmin: 'error',
        weather_link: 'error'
      }
  },
  mounted: function(){
    this.$nextTick(function(){
      this.get_weather()
    })
  },
  methods: {
    get_weather: 
    function(){
      this.$http.get('http://apis.skplanetx.com/weather/current/minutely?'+getXY(),{
        headers: {
          appKey : 'd81348e0-a70e-3928-a8b5-9a35108a90c2' 
        }
      }).then((result) => {
        console.log('날씨정보\n');
        console.log(result);
        this.station_name = result.data.weather.minutely[0].station.name;
        this.sky = result.data.weather.minutely[0].sky.code;
        this.skyname = result.data.weather.minutely[0].sky.name;
        this.tc = result.data.weather.minutely[0].temperature.tc;
        this.tmax = result.data.weather.minutely[0].temperature.tmax;
        this.tmin = result.data.weather.minutely[0].temperature.tmin;
        this.humidity = result.data.weather.minutely[0].humidity;
      })
      this.$http.get('http://apis.skplanetx.com/weather/dust?'+getXY(),{
        headers: {
          appKey : 'd81348e0-a70e-3928-a8b5-9a35108a90c2' 
        }
      }).then((result) => {
        console.log('미세먼지\n');
        console.log(result);
        this.dust_value = result.data.weather.dust[0].pm10.value;
        this.dust_grade = result.data.weather.dust[0].pm10.grade;
      })
      this.$http.get('http://apis.skplanetx.com/weather/summary?'+getXY(),{
        headers:{
          appKey : 'd81348e0-a70e-3928-a8b5-9a35108a90c2' 
        }
      }).then((result) => {
        console.log('간편날씨\n');
        console.log(result)
        this.tomorrow_sky_code = result.data.weather.summary[0].tomorrow.sky.code;
        this.tomorrow_tmax = result.data.weather.summary[0].tomorrow.temperature.tmax;
        this.tomorrow_tmin = result.data.weather.summary[0].tomorrow.temperature.tmin;
      })
      }
      
    }
  }
</script>
<style scoped>
  #refresh{
    width: 2.5vh;
    position: absolute;
    top:2vh;
    right: 1.5vw;
  }
  .weather_icon{
    margin-top: 2.5vh;
    position: relative;
    left: 1vw;
    width: 15vh;
    height: 15vh;
    background-size: 100%;
    display: flex;
    justify-content: center;
  }
  .cloud_many{
    background-image: url('../assets/cloud-many.png');
  }
  .rain{
    background-image: url('../assets/drop.png');
  }
  .fog{
    background-image: url('../assets/fog.png');
  }
  .sun{
        background-image: url('../assets/sun.png');
  }
  .snow{
    background-image: url('../assets/snow.png');
  }
  .rain-snow{
    background-image: url('../assets/rain_snow.png');
  }
  #station{
    position: absolute;
    top: 11.5vh;
    left: 12vw;
    font-size: 2.5vmax;
  }
  .weather_icon span{
    position: relative;
    top:15vh;
    font-size: 1vmax;
  }
  #weather_temperature{
    margin-top: 4vh;
    text-align: left;
    font-size: 1.1vmax;
  }
  #tc{
    color: #FF9900;
  }
  .tmax{
    margin-right: 0.8vw;
    color: #FF3939;
  }
  .tmin{
    
    color: #0088FF;
    
  }

  #weather_temperature div{
    margin-top: 1vh;
  }
  #weather_temperature #humidity_dust{
    margin-top: 3vh;
  }
  #humidity{
    color: #66A1F9;
  }
  #dust{
    color: #9B9B47;
  }
  #tomorrow{
    display: flex;
    align-items: center;
  }
  #tomorrow_title{
    font-size: 1.5vmax;
    margin-top: 5vh;
    margin-bottom: 3vh;
  }
  .tomorrow_weather_icon{
    margin-top: 2.5vh;
    position: absolute;
    right: 1vw;
    width: 10vh;
    height: 10vh;
    background-size: 100%;
  }
  #tommorrow_temperature{
    display: inline;
    font-size: 1.1vmax;
  }
  a:link{
    color: black;
  }
  a:visited{
    color: black;
  }
  a:hover{
    color: black;
  }
  #link-box{
    position: relative;
    top: 4vh;
  }
  #link-box a{
    position: absolute;
    right: 0.5vw;
    font-size: 1vmax;
  }
</style>