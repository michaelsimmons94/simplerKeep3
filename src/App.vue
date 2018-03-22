<template>
  <v-app>
    <div id="app">
    <h1>Even Simpler Keep</h1>
    <br>
    <h3>A more bayzic version of Google Keep</h3>
    <router-view/>
  </div>
  </v-app>
</template>

<script>
export default {
 
  name: 'App'
}
</script>
<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-color: dodgerblue;
}
h1{
  font-family: 'Pacifico', cursive;
  color:white;
  font-size: 4em;
}
h3{
  color:white;
  font-size: 1.5em;
}
</style>
