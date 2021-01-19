<template>
  <Header/>
  <LogUpload/>
  <div class="circle"></div>
  <div class="square"></div>
</template>

<script>
import LogUpload from './components/LogUpload.vue'
import Header from './components/Header.vue'

export default {
  name: 'App',
  components: {
    LogUpload, 
    Header
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300&display=swap');
body{
background: #fbfbff;
font-family: 'Poppins', sans-serif;
}
#app {
  font-family:  'Poppins', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
