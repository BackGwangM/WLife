<template>
  <div id="weather">
    <img src="../assets/refresh-button.png" alt="새로고침" v-on:click="get_weather" id="refresh"><br>
    <span v-if="msg!='날씨를 불러와 주세요'||sky!='error'">현재 {{station_name}}의 날씨는 {{sky}}상태이며 미세먼지 농도는 {{dust_value}}㎍/㎥으로써 {{dust_grade}}수준입니다.</span>
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
        sky : 'error',
        msg: '날씨를 불러와 주세요',
        dust_value: 'error',
        dust_grade: 'error'
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
          appKey : '---' 
        }
      }).then((result) => {
        console.log('날씨정보\n');
        console.log(result);
        this.station_name = result.data.weather.minutely[0].station.name;
        this.sky = result.data.weather.minutely[0].sky.code;
      })
      this.$http.get('http://apis.skplanetx.com/weather/dust?'+getXY(),{
        headers: {
          appKey : '---' 
        }
      }).then((result) => {
        console.log('미세먼지\n');
        console.log(result);
        this.dust_value = result.data.weather.dust[0].pm10.value;
        this.dust_grade = result.data.weather.dust[0].pm10.grade;
      })
      this.msg = 'success';
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
</style>