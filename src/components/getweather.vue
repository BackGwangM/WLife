<template>
  <div id="weather">
    <button v-on:click="searchTerm">날씨 불러오기</button><br>
    <span>{{msg}}</span>
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
        msg: '날씨를 불러와 주세요'
      }
  },
  methods: {
    searchTerm: function(){
      this.$http.get('http://apis.skplanetx.com/weather/current/minutely?'+getXY(),{
        headers: {
          appKey : 'd81348e0-a70e-3928-a8b5-9a35108a90c2' 
        }
      }).then((result) => {
        console.log(result);
        alert(result.data.weather.minutely[0].sky.name);
      })
      }
      
    }
  }
</script>
<style scoped>

</style>

