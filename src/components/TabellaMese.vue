<template>
    <div id="TabellaMese">
        <div>
            <button @click="indietroMese">&lt;</button>
            <span class="mese">{{filtraMese(vistaCorrente.mese).mese}}</span>
            <button @click="avantiMese">&gt;</button>
            <span class="spazio"></span>
            <button @click="indietroAnno">&lt;</button>
            <span class="anno">{{vistaCorrente.anno}}</span>
            <button @click="avantiAnno">&gt;</button>
        </div>
        <table>
            <thead>
                <tr>
                    <td :key="giorno.id" :style="coloraFestivi(giorno)" v-for="giorno in giorni">
                        <p>{{giorno.valore}}</p>
                        <p>{{giorno.giornoSettimana}}</p>
                    </td>
                </tr>
            </thead>
            <tbody>
                <tr :key="voce.id"  v-for="voce in nomeVoci">
                    <td :key="giorno.id" :style="coloraFestivi(giorno)" v-for="giorno in giorni">{{ stampaCella(voce,giorno) }}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
export default {
    name: 'TabellaMese',
    props: [
        'mese', // 12
        'anno', // 2020
        'righe',// " `Mario ROSSI` ,`Maria VERDI` ,`Luigi BIANCHI`"
        'note'  // "`Mario ROSSI` `12-10-2020` `PR`, `Luigi BIANCHI` `01-10-2020` `AS`"
        ],
    data : function(){
        return {
            vistaCorrente : { anno : Number(this.anno), mese : Number(this.mese) },
            giorni :    [], // {id : 2, valore : '02', giornoSettimana : 'L'}
            nomeVoci :  [], // {id : 3, nome : 'Mario ROSSI'}
            eventi :    [], // {giorno : 4, mese : 12, anno : 2020, voce : 'Mario ROSSI', evento : 'Assenza'}
            tabella :   [], // array bidimensionale
            infoMese : [
                {id : 1, mese : 'Gennaio',  giorni : 31},
                {id : 2, mese : 'Febbraio', giorni : 28}, // * eccetto il bisestile
                {id : 3, mese : 'Marzo',    giorni : 31},
                {id : 4, mese : 'Aprile',   giorni : 30},
                {id : 5, mese : 'Maggio',   giorni : 31},
                {id : 6, mese : 'Giugno',   giorni : 30},
                {id : 7, mese : 'Luglio',   giorni : 31},
                {id : 8, mese : 'Agosto',   giorni : 31},
                {id : 9, mese : 'Settembre',giorni : 30},
                {id : 10,mese : 'Ottobre',  giorni : 31},
                {id : 11,mese : 'Novembre', giorni : 30},
                {id : 12,mese : 'Dicembre', giorni : 31}
            ],
            infoGiorni : [
                {id : 1, giorno : 'Lunedì',     sigla : 'L'},
                {id : 2, giorno : 'Martedi',    sigla : 'M'},
                {id : 3, giorno : 'Mercoledi',  sigla : 'M'},
                {id : 4, giorno : 'Giovedì',    sigla : 'G'},
                {id : 5, giorno : 'Venerdì',    sigla : 'V'},
                {id : 6, giorno : 'Savato',     sigla : 'S'},
                {id : 0, giorno : 'Domenica',   sigla : 'D'}
            ],
            infoFestivi : [
                {giorno : 1, mese : 1, festa : 'Capodanno'},
                {giorno : 6, mese : 1, festa : 'Epifania'},
                {giorno : 0, mese : 0, festa : 'Pasqua?????'},
                {giorno : 0, mese : 0, festa : 'Pasquetta?????'},
                {giorno :25, mese : 4, festa : 'Liberazione'},
                {giorno : 1, mese : 5, festa : 'F. del Lavoro'},
                {giorno : 2, mese : 6, festa : 'F. della Repubblica'},
                {giorno :15, mese : 8, festa : 'Ferragosto'},
                {giorno : 1, mese :11, festa : 'Tutti i Santi'},
                {giorno : 8, mese :12, festa : 'Immacolata Concezione'},
                {giorno :25, mese :12, festa : 'Natale'},
                {giorno :26, mese :12, festa : 'S. Stefano'}
            ]
        }
    },
    methods : {
        /**
         * Determina il giorno della settimana
         */
        giornoSettimana : function(aaaa,mm,gg){
            const t = [0, 3, 2, 5, 0, 3, 5, 1, 4, 6, 2, 4];
            if(aaaa > 1752){
                aaaa -= (mm < 3) ? 1 : 0;
                let valore = (aaaa + Math.trunc(aaaa/4) - Math.trunc(aaaa/100) + Math.trunc(aaaa/400) + t[mm-1] + gg) % 7;
                const risultato = this.infoGiorni.filter((r) => { return r.id == Math.trunc(valore)});
                if(risultato && risultato.length > 0)
                    return risultato[0].sigla;
            }
            return "";
        },
        /**
         * Se bisestile restituisce un valore numerico 1, altrimenti 0.
         * @return {number}
         */
        annoBisestile : function(annoX){
            return (annoX % 400 === 0 || (annoX % 4 === 0 && annoX % 100 !== 0)) ? 1 : 0;
        },
        giorniMese : function(meseX,annoX){
            this.giorni = [];
            this.giorni.push({id : 0, valore : null, giornoSettimana : null});
            let limite = this.filtraMese(meseX).giorni + this.annoBisestile(annoX);
            for(let i=1; i <= limite; i++){
                let giorno = {
                    id : i,
                    valore : i < 10 ? '0'+i : ''+i,
                    giornoSettimana : this.giornoSettimana(annoX,meseX,i)
                }
                this.giorni.push(giorno);
            }
        },
        filtraMese : function(meseID){
            let listLimite = this.infoMese.filter( (r)=>{return r.id == meseID} );
            if(listLimite.length > 0){
                return listLimite[0];
            }
        },
        creaNomiVoci : function(){
            const lista = this.righe.split(',');
            let indice=0;
            for(const voce of lista){
                const stringa = this.identificaStringa("`",voce);
                if(stringa != null){
                    this.nomeVoci.push({id : indice, nome : stringa.valore});
                }
                indice++;
            }
        },
        /**
         * Estrae una stringa di un testo compresa tra due caratteri 'estemi'
         * 
         * @param {string} carattere  es.:  "`"
         * @param {string} testo 
         * @return {object|null} {valore : "Mario ROSSI", idFine : 39}
         */
        identificaStringa: function(carattere,testo){
            let p1 = testo.indexOf(carattere);
            let p2 =0;
            if(p1 >= 0){
                p2 =  testo.indexOf(carattere,p1+1);
                if(p2 > 0){
                    testo.substring(p1+1,p2);
                    return {valore : testo.substring(p1+1,p2), idFine : p2 + 1}
                }
            }
            return null;
        },
        inizializzaCelle : function(){
            this.tabella = [];
            for(let row=0; row < this.nomeVoci.length; row++){
                let riga = [];
                riga.push(this.nomeVoci[row].nome);
                for(let col=1; col < this.giorni.length; col++){
                    // aggiungi evento 
                    riga.push(this.evento( 
                        this.nomeVoci[row].nome,      // voce
                        col,                          // gg
                        this.vistaCorrente.mese,      // mm
                        this.vistaCorrente.anno       // aaaa
                    ));
                }
                this.tabella.push(riga);
            }
        },
        /**
         * Elabora le informazioni provenienti dalla props 'note' e le inserisce nell'array
         * 'this.eventi'
         */
        inizializzaListaEventi: function(){
            let sEventi = this.note.split(',');
            for(let stringa of sEventi){
                let p = 0;
                let voce = this.identificaStringa("`",stringa);
                if(voce != null){
                    p = voce.idFine;
                    let data = this.identificaStringa("`",stringa.substring(p));
                    if(data != null && data.valore.length == 10){
                        p += data.idFine;
                        let evento = this.identificaStringa("`",stringa.substring(p));
                        if(evento != null){
                            this.eventi.push({
                                giorno  : Number(data.valore.substring(0,2)), 
                                mese    : Number(data.valore.substring(3,5)), 
                                anno    : Number(data.valore.substring(6,10)), 
                                voce    : voce.valore, 
                                evento  : evento.valore
                            });
                        }
                    }
                        
                    
                }else{
                    continue;
                }
                   
            }
            
        },
        /**
         * Estrae dall' array 'this.eventi' la proprietà evento associata a una voce 
         * e alla data di una tabella.
         * 
         * @param {string} voce 
         * @param {number} giorno
         * @param {number} mese
         * @param {number} anno
         */
        evento : function(voce,giorno,mese,anno){
            const risultato = this.eventi.filter((r) => {
                return r.giorno === giorno && r.mese === mese && r.anno == anno && r.voce == voce
            });
            if(risultato && risultato.length > 0){
                return risultato[0].evento;
            }
            return '';
        },
        stampaCella : function(nomeVoce,giorno){
            for (const riga of this.tabella) {
                if(riga[0] == nomeVoce.nome)
                    return riga[giorno.id];
            }
        },
        /**
         * Applica uno stile alla cella di una tabella.
         * @param {object} giorno es.: {id : 2, valore : '02', giornoSettimana : 'L'}
         * @return {object} strile CSS
         */
        coloraFestivi : function(giorno){
            const risultato = this.infoFestivi.filter((r) => { 
                return r.giorno === giorno.id && r.mese === this.vistaCorrente.mese
            });
            let superfestivo = risultato && risultato.length > 0 ? true : false;
            
            if(giorno.giornoSettimana == 'D' || superfestivo){ 
                return {
                    'background-color' : 'grey',
                    'color' : 'white'
                }
            }else if(giorno.giornoSettimana == 'S'){ 
                return {
                    'background-color' : 'lightgrey'
                }
            }
            
        },
        avantiMese : function(){
            this.vistaCorrente.anno = this.vistaCorrente.mese < 12 ? this.vistaCorrente.anno : ++this.vistaCorrente.anno;
            this.vistaCorrente.mese = this.vistaCorrente.mese < 12 ? this.vistaCorrente.mese + 1 : 1;
            this.giorniMese(this.vistaCorrente.mese,this.vistaCorrente.anno);
            this.inizializzaCelle();
        },
        indietroMese : function(){
            this.vistaCorrente.anno = this.vistaCorrente.mese > 1 ? this.vistaCorrente.anno : --this.vistaCorrente.anno;
            this.vistaCorrente.mese = this.vistaCorrente.mese > 1 ? this.vistaCorrente.mese - 1 : 12;
            this.giorniMese(this.vistaCorrente.mese,this.vistaCorrente.anno);
            this.inizializzaCelle();
        },
        avantiAnno : function(){
            this.vistaCorrente.anno++;
            this.giorniMese(this.vistaCorrente.mese,this.vistaCorrente.anno);
            this.inizializzaCelle();
        },
        indietroAnno : function(){
            this.vistaCorrente.anno--;
            this.giorniMese(this.vistaCorrente.mese,this.vistaCorrente.anno);
            this.inizializzaCelle();
        }
    },
    
    mounted : function(){
        this.giorniMese(this.vistaCorrente.mese,this.vistaCorrente.anno);
        this.creaNomiVoci();
        this.inizializzaListaEventi();
        this.inizializzaCelle();
    }
}
</script>

<style scoped>
p{
    margin: 0;
    padding: 0;
    line-height: auto;
}
button{
    font-size: 2em;
    font-weight: bolder;
}
span{
    display: inline-block;
    font-size: 2em;
    font-weight: bolder;
    color: black;
}
span.mese{
    width: 6em;
}
span.anno{
    width: 3em;
}
span.spazio{
    width: 1em;
}
table{
    width: 100%;
    border-collapse: collapse;
}
table td{
    border: 1px solid #222;
}
</style>