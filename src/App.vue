<template>
  <div id="app">
    <barra-menù colore="white" sfondo="black">
      <menù nome="Animali" >
        <voce-menù nome="Pesci" link="#" colore="blue" sfondo="red"/>
        <voce-menù nome="Anfibi" />
        <menù nome="Uccelli">
          <voce-menù nome="Rondini" />
          <voce-menù nome="Aquile" />
        </menù>
        <menù nome="Mammiferi">
        <menù nome="Carnivori">
          <voce-menù nome="Canidi" />
          <menù nome="Felini" colore='red' sfondo='grey'>
            <voce-menù nome="Tigre" />
            <voce-menù nome="Gatto" />
            <voce-menù nome="Leone" />
          </menù>
          <voce-menù nome="Ursidi" />
        </menù>
        </menù>
      </menù> 
      <voce-menù nome="Piante" />
      <voce-menù nome="Funghi" />
    </barra-menù>
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
import Menu from './components/Menu.vue'
import VoceMenu from './components/VoceMenu.vue'
import BarraMenu from './components/BarraMenu.vue'


export default {
  name: 'App',
  components: {
    'tabella-mese' : TabellaMese,
    'login' : Login,
    'tabella' : Tabella,
    'diagramma' : DiagrammaXY,
    'evento' : Evento,
    'menù' : Menu,
    'voce-menù' : VoceMenu,
    'barra-menù' : BarraMenu
  }
}
</script>

<style>
#app {
  
}
</style>
