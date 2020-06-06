<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <login hostServer="http://localhost:8080/">
    </login>
    <tabella-mese mese="11" anno="2020" 
                  righe=" `Mario ROSSI` ,`Maria VERDI` ,`Luigi BIANCHI`"
                  note="`Mario ROSSI` `12-11-2020` `PR`, `Luigi BIANCHI` `01-10-2020` `AS`">
    </tabella-mese>
    <tabella  colonne="`CAP`,`Città`,                `Prov.`,  `Regione` "
              righe="`80100` `<b>NAPOLI</b>`          `NA`     `CAMPANIA` , 
                     `81100` `CASERTA`                `CE`     `CAMPANIA` ,
                     `81024` `MADDALONI`              `CE`     `CAMPANIA` ,
                     `80053` `CASTELLAMARE DI STABIA` `NA`     `CAMPANIA`  ,
                     `32020` `LENTIANI`               `BL`     `VENETO` ,
                     `90049` `CITTA' DELLA PIEVE`     `PG`     `UMBRIA` ,
                     `89022` `CITTANOVA`              `RC`     `CALABRIA`,
                     `00120` `CITTA' DEL VATICANO`    `SCV`    `STATO VATICANO`, 
                     `13046` `LIVORNO FERRARIS`       `VC`     `PIEMONTE`, 
                     `00100` `<b>ROMA</b>`            `RM`     `LAZIO`, 
                     `46040` `CASALROMANO`            `MN`     `LOMBARDIA`, 
                     `20100` `<b>MILANO</b>`          `MI`     `LOMBARDIA`"
    ></tabella>
    <diagramma funzione="x*x*x-3*x-1/2" altezza="550" lunghezza="750" unita="90"></diagramma>
    <evento nome="Assenza">
      <template v-slot:pulsante1>
        <input type="button" value="Salva"/>
      </template>
      <template v-slot:pulsante2>
        <input type="button" value="Cancella"/>
      </template>
    </evento>
  </div>
</template>

<script>
import Login from './components/Login.vue'
import TabellaMese from './components/TabellaMese.vue'
import Tabella from './components/Tabella.vue'
import DiagrammaXY from './components/DiagrammaXY.vue'
import Evento from './components/Evento.vue'

export default {
  name: 'App',
  components: {
    'tabella-mese' : TabellaMese,
    'login' : Login,
    'tabella' : Tabella,
    'diagramma' : DiagrammaXY,
    'evento' : Evento
  }
}
</script>

<style>
#app {
  
}
</style>
