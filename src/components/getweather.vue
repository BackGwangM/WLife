<template>
  <div id="weather">
    <button v-if="msg=='날씨를 불러와 주세요'" v-on:click="get_weather">날씨 불러오기</button><br>
    <span v-if="msg!='날씨를 불러와 주세요'">현재 {{station_name}}의 날씨는 {{sky}}이며 미세먼지 농도는 {{dust_value}}㎍/㎥으로써 {{dust_grade}}수준입니다.</span>
    <div id="weather"></div>
  </div>
  
</template>
<script>
var lat, lon;
if(navigator.geolocation){
    navigator.geolocation.getCurrentPosition(function(position){
        lat = position.coords.latitude;
        lon = position.coords.longitude;
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
  var param = 'lat='+lat+'&lon='+lon+'&version=1';
  return param;
  }
export default {
  data(){
      name: 'Get-Weather'
      return{
        station_name : '권한 설정이 안됬거나 지원되지 않는 브라우저 입니다.',
        sky : '새로고침이 필요합니다.',
        msg: '날씨를 불러와 주세요',
        dust_value: '미세먼지가 조회가 되지 않습니다.',
        dust_grade: '미세먼지가 조회가 되지 않습니다.'
      }
  },
  methods: {
    get_weather: function(){
      this.$http.get('http://apis.skplanetx.com/weather/current/minutely?'+getXY(),{
        headers: {
          appKey : 'd81348e0-a70e-3928-a8b5-9a35108a90c2' 
        }
      }).then((result) => {
        console.log('날씨정보\n');
        console.log(result);
        this.station_name = result.data.weather.minutely[0].station.name;
        this.sky = result.data.weather.minutely[0].sky.name;
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
      this.msg = '불러와 졌습니다.';
      }
      
    }
  }
</script>
<style scoped>

</style>