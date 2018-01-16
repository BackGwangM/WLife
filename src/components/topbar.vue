<template>
    <div id="topbar">
        <span id="title">W Life</span>
        <div id="menu_box">
            <div class="menu long">To-do List</div>
            <div class="menu">Weather</div>
            <div class="menu long">I am BGM</div>
        </div>
    </div>
    
</template>
<script>
export default {
  
}
</script>
<style scoped>
#topbar{
    background-color: rgba(0, 0, 0, 0.5);
    width: 100%;
    height: 10vh;
    padding-left: 3vw;
    color: white;
    display: flex;
    align-items: center;
}
#title{
    font-size: 5vmin;
    font-weight: 900;
    color: #ABFFFF;
}
#menu_box{
    position: relative;
    left: 5vw;
    font-size: 4vmin;   
}
.menu{
    display: inline;
    margin-left: 15vw;
    text-decoration: none;
  position: relative;
}
.menu:after {
  content: '';
  position: absolute;
  bottom: -2.75vh;
  left: -0.8vw;
  width: 0%;
  border-left: 0.5vmin solid transparent;
  border-right: 0.5vmin solid transparent;
  border-bottom: 0.5vmin solid #FFFFFF;
  transition: 0.4s;
}
.menu:hover:after {
    width: 9vw;
}

.long.menu:hover:after {
    width: 10vw;
}
</style>
