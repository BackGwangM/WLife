<template>
  <div id="app">
    <weather />
  </div>
  
</template>

<script>
import getweather from './components/getweather'

export default {
  name: 'app',
  components: {
    'weather':getweather
  }
}
</script>

<style>
</style>
