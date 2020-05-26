<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld  msg="Ciao Mondo ^_^"/>
    <tabella-mese mese="11" anno="2020" 
                  righe=" `Mario ROSSI` ,`Maria VERDI` ,`Luigi BIANCHI`"
                  note="`Mario ROSSI` `12-11-2020` `PR`, `Luigi BIANCHI` `01-10-2020` `AS`">
    </tabella-mese>
  </div>
</template>

<script>
import TabellaMese from './components/TabellaMese.vue'

export default {
  name: 'App',
  components: {
    'tabella-mese' : TabellaMese
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
