<template>
    <div id="Menu" ref="menu" :style="stileMenu">
        <div :id="nome" style="height:100%;display:inline;color:inherit;background:inherit;cursor:pointer;" @click="eventoClic">{{nome}} <span>{{freccia}}</span></div>
        <template v-if="listaVisibile">
            <ul :style="stile">
                <li><slot></slot></li>
            </ul>
        </template>
        <!-- creato solo per poter implementare il metodo di calcolo della lunghezza del testo -->
        <canvas ref="canvas" width="0" height="0" style="display:none"></canvas>
    </div>
</template>

<script>


export default {
    name : 'Menu',
    props : ['nome','colore','sfondo'],
    data : function(){
        return {
            livelloMenu : 0,
            freccia : ''
        }
    },
    
    methods : {
        eventoClic : function(e){
            if(e.target.id === this.nome){ 
                // la commutazione avviene solo per l'elemento "Menu" che lo ha generato
                this.$store.dispatch('aggiungiMenuCorrente', {menu: this.nome, livello:this.livelloMenu-1});
                
            }
        },
         lunghezzaTesto : function (text, font) {
            // re-use canvas object for better performance
            var canvas =  this.$refs.canvas;
            var context = canvas.getContext("2d");
            context.font = font;
            var metrics = context.measureText(text);
            return metrics.width;
        },
        lunghezzaMassima : function() {
            var max = 0;
            if(this.$refs.menu){
                if(this.$refs.menu.parentElement.tagName == "LI"){
                    var lista = this.$refs.menu.parentElement;
                    let riga = lista.firstChild; // div
                    while(riga){
                        let link = riga.firstChild; // a
                        if(max < this.lunghezzaTesto(link.textContent + ' X',"12px Arial"))
                            max = this.lunghezzaTesto(link.textContent + ' X',"12px Arial"); // lunghezza stringa voce
                        riga = riga.nextSibling;
                    }
                    return (max/12.0) + 'em';
                }
            }
            return '100%';
        },
        livello : function(){
            var indice=0;
            if(this.$refs.menu){
                let nodoGenitore = this.$refs.menu.parentElement;
                if(nodoGenitore){
                    while(nodoGenitore.tagName == "LI"){
                        indice++;
                        nodoGenitore = nodoGenitore.parentElement.parentElement.parentElement;
                    }
                }
            }
            return indice;
        }
    },

    computed : {
        stileMenu : function(){
            return {
                '--color' : this.colore,
                '--background' : this.sfondo,
                '--color-hover' : this.sfondo,
                '--background-hover' : this.colore
            };
        },
        stile : function(){
            // se è il primo livello, il menu camparirà sotto
            if(this.$refs.menu.parentElement.tagName != "LI")// se <span>
                return {
                    position: 'absolute',
                    top: '2em',
                    left: '0em',
                    width: 'auto',
                    display: 'inline-block',
                    'list-style-type': 'none',
                    margin: 0,
                    padding: 0
                };
            else // se è un sottolivello, il menu comparirà a destra
                return {
                    position: 'absolute',
                    top: 0,
                    left: this.lunghezzaMassima(),
                    display: 'block',
                    'list-style-type': 'none',
                    margin: 0,
                    padding: 0
                };
        },
        /**
         * Rende visibile i menu della voce selezionata (e quello dei genitori)
         * N.B.: utilizzo della libreria 'Vuex'
         */
        listaVisibile : function(){
            // [1] menu posti nello stesso livello di quello corrente
            if(this.$store.getters.livelloMenu +1 == this.livelloMenu){
                return this.nome == this.$store.getters.menuSelezionato;
            }

            // [2] menu posti nei livelli diversi da qullo corrente
            var oContext = this;
            // si impedisce l'eliminazione dei menu genitori di quello corrente
            return this.$store.getters.sequenzaMenu.find((sVoce)=>{return sVoce == oContext.nome}) != null;
        }
    },
    mounted : function(){
        this.livelloMenu = this.livello()+1;
        
        // se è il primo livello, comparirà la freccia ▼
        if(this.$refs.menu.parentElement.tagName != "LI")// se <span>
            this.freccia = '▼';
        else // se è un sottolivello, comparirà la freccia ►
            this.freccia = '►';
        
    },
    watch : {
        
    }

}
</script>

<style scoped>
#Menu{
    font-family:"Arial";
    font-size: 14px;
    position: relative;
    display: block;
    white-space: nowrap;
    padding:0.4em;
    border-top: 1px solid gray;
}

div{
    color : var(--color);
    background: var(--background);
}
div:hover{
    color : var(--color-hover);
    background: var(--background-hover);
}

li{
    margin: 0;
    padding: 0;
    border: 1px solid gray !important;
    width:max-content;
}
span{
    padding-left: 0.5em;
    float:right ; 
    cursor:default;
}
</style>