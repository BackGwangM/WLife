<template>
  <div id="tile">
      <div class="card">
          <getweather />
      </div>
  </div>
</template>
<script>
import getweather from './getweather'
export default {
components:{
    'getweather' : getweather
}

}
</script>
<style scoped>
#tile{
    position: absolute;
    top:25vh;
    left: 15vw;
    display: inline;
}
    .card{
        position: relative;
        padding-left: 1vw;
        padding-right: 1vw;
        padding-top: 3vh;
        top:2vh;
        width: 18vw;
        height: 47vh;
        background-color: white;
    }
    .card:before{
        content: "";
        position: absolute;
        top: -2vw;
        left: 0;
        border-bottom: 2vw solid white;
        border-left: 2vw solid transparent;
        border-right: 2vw solid white;
        width: 16vw;
        height: 0;
    }
    .card:after {
        content: "";
        position: absolute;
        bottom: -2vw;
        left: 0;
        border-top: 2vw solid white;
        border-left: 2vw solid white;
        border-right: 2vw solid transparent;
        width: 16vw;
        height: 0;
    }
</style>
