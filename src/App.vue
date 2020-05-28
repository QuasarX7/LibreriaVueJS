<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <login hostServer="http://localhost:8080/">
    </login>
    <tabella-mese mese="11" anno="2020" 
                  righe=" `Mario ROSSI` ,`Maria VERDI` ,`Luigi BIANCHI`"
                  note="`Mario ROSSI` `12-11-2020` `PR`, `Luigi BIANCHI` `01-10-2020` `AS`">
    </tabella-mese>
  </div>
</template>

<script>
import Login from './components/Login.vue'
import TabellaMese from './components/TabellaMese.vue'

export default {
  name: 'App',
  components: {
    'tabella-mese' : TabellaMese,
    'login' : Login
  }
}
</script>

<style>
#app {
  
}
</style>
