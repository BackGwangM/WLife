<template>
    <div id="topbar">
        <span id="title">W Life</span>
        <div id="menu_box">
            <span class="menu">To-do List</span>
            <span class="menu">Weather</span>
            <span class="menu">I am BGM</span>
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
    font-size: 2.5vmax;
    font-weight: 900;
    color: #ABFFFF;
}
#menu_box{
    position: relative;
    left: 5vw;
    font-size: 2vmax;   
}
.menu{
    margin-left: 15vw;
}
</style>
