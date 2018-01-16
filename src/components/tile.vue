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
    top:20vh;
    left: 15vw;
    display: inline;
    animation-name: uptile;
    animation-duration: 0.5s;
}
    .card{
        position: relative;
        padding-left: 1vw;
        padding-right: 1vw;
        padding-top: 3vh;
        top:2vh;
        width: 18vw;
        height: 60vh;
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
    @keyframes uptile{
        from{
            opacity: 0;
            top:50vh;
            left: 0vw;
        }
        75%{
            top:17.5vh;
            left: 17.5vw;
        }
        to{
            opacity: 1;
            top: 20vh;
            left: 15vw;
        }
    }
</style>
