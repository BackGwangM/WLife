<template>

  <div id="app">
    <topbar/>
    <weather />
  </div>
  
</template>

<script>
import getweather from './components/getweather'
import topbar from './components/topbar'

export default {
  name: 'app',
  components: {
    topbar,
    'weather':getweather
  }
}
</script>

<style>
@import url(https://cdn.rawgit.com/moonspam/NanumSquare/master/nanumsquare.css);
body{
  font-family: 'NanumSquare';
  margin: 0 0 0 0;
  background-image: url(https://images.unsplash.com/photo-1468276311594-df7cb65d8df6?auto=format&fit=crop&w=1950&q=80);
  background-position: center center;
  background-attachment: fixed;
  background-size: 100%;
  background-repeat: no-repeat;
  overflow-x: hidden;
}
#app{
  background-color: rgba(0, 0, 0, 0.3);
    width: 100%;
    height: 100vh;
}
</style>
