<template>
    <table border="1">
        <thead>
            <tr>
                <th :key="colonna.id" v-for="colonna in titoliColonne">
                    <span>{{colonna.nome}}</span>
                    <button @click="ordina(colonna)" >{{colonna.ordine}}</button>
                </th>
            </tr>
            <tr>
                <td class="input" :key="colonna.id" v-for="colonna in titoliColonne">
                    <input type="search" placeholder="cerca" @keyup="filtra($event,colonna)">
                </td>
            </tr>
        </thead>
        <tbody>
            <tr :key="id" v-for="(riga, id) in tabella">
                <td :key="idC" v-for="(cella,idC) in riga" v-html="cella"></td>
            </tr>
        </tbody>
    </table>
</template>

<script>
export default {
    name : 'Tabella',
    props : ['colonne','righe'],
    data : function(){
        return {
            titoliColonne : [], // {id : 0, nome : 'CAP', ordine : '↔' }
            tabella : []
        }
    },
    methods : {
        /**
         * Determina il nome delle colonne.
         */
        determinaTitoliColonne : function(){
            this.titoliColonne = [];
            const colonne = this.colonne.split(',');
            let indice=0;
            for(const colonna of colonne){
                const stringa = this.identificaStringa("`",colonna);
                if(stringa != null){
                    this.titoliColonne.push({id : indice, nome : stringa.valore, ordine : '⚪'});
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
        costruisciTabella : function(){
            this.tabella = [];
            const righe = this.righe.split(',');
            
            for(const riga of righe){
                //`80100` `NAPOLI`                 `NA`     `CAMPANIA`
                const rigaTab = [];
                let p=0;
                while(p < riga.length){
                    const cella = this.identificaStringa('`',riga.substring(p));
                    if(cella){
                        rigaTab.push(cella.valore);
                        p += cella.idFine;
                    }else
                        break;
                }
                if(rigaTab.length > 0){
                    this.tabella.push(rigaTab);
                }
                
            }
        },
        ordina : function(colonna){
            for(let id in this.titoliColonne){
                if(this.titoliColonne[id].id === colonna.id){
                    let crescente = null;
                    switch(this.titoliColonne[id].ordine){
                        case '⚪' :
                        case '▲' : 
                            this.titoliColonne[id].ordine = '▼';
                            crescente = true;
                            break;
                        case '▼' : 
                            this.titoliColonne[id].ordine = '▲';
                            crescente = false;
                            break;
                        default :
                            this.titoliColonne[id].ordine = '⚪';
                            break;
                    }
                    // ordina tabella per id (colonna)
                    this.tabella.sort((a,b)=>{
                        let vA = a[id] ? a[id].toLowerCase() : '';
                        let vB = b[id] ? b[id].toLowerCase() : '';
                        
                        if(crescente != null){
                            if(vA < vB) return crescente ? -1 :  1;
                            if(vA > vB) return crescente ?  1 : -1;
                        }
                        return 0;
                    });
                }
                else
                    this.titoliColonne[id].ordine = '⚪';
            }
        },
        filtra : function($event,colonna){
            this.determinaTitoliColonne();
            this.costruisciTabella();
            this.tabella = this.tabella.filter((riga)=>{
                return riga[colonna.id].toLowerCase().includes( $event.target.value.toLowerCase() )
            });
        }
    },
    mounted : function(){
        this.determinaTitoliColonne();
        this.costruisciTabella();
    }
    
}
</script>


<style scoped>
table{
    table-layout: auto;
    margin: 1em;
    border-collapse: collapse;
    font-family: Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
}
th,td{
    border: 1px solid #222;
    padding-left: 1em;
    padding-right: 1em;
}
td.input{
    margin: 1px;
    padding: 2px;
    background: lightgrey;
}
td.input > input{
    width: 100%;
}
th > span{
    display: inline-block;
    padding-top: 0.25em;
    padding-bottom: 0.25em;
}
th {
    border: 1px solid #222;
    background: darkgrey;
    position:relative;
    padding-left: 2em;
    padding-right: 2em;
}
th button{
    font-weight: bold;
    position: absolute;
    width: 1.8em;
    height: 100%;
    text-align: left;
    margin: 0;
    padding-left: 0.2em;
    right: 0;
}
</style>
