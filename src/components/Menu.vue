<template>
    <div id="Menu" ref="menu" :style="stileEtichetta">
        <div :id="nome" style="height:100%;display:inline" @click="eventoClic">{{nome}}</div>
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
    props : ['nome'],
    data : function(){
        return {
            livelloMenu : 0
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
                        if(max < this.lunghezzaTesto(link.textContent,"12px Arial"))
                            max = this.lunghezzaTesto(link.textContent,"12px Arial"); // lunghezza stringa voce
                        riga = riga.nextSibling;
                    }
                    return (max/12.0 + 0.77) + 'em';
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
        stileEtichetta : function() {
            return {
                position: 'relative',
                display: 'block',
                'white-space': 'nowrap',
                padding:'0.4em',
                'border-top': '1px solid gray',
                cursor: 'pointer'
            };
        },
        stile : function(){
            // se è il primo livello, il menu camparirà sotto
            if(this.$refs.menu.parentElement.tagName != "LI")// se <span>
                return {
                    position: 'absolute',
                    top: '2em',
                    left: '0em',
                    background: 'white',
                    display: 'block',
                    'list-style-type': 'none',
                    margin: 0,
                    padding: 0
                };
            else // se è un sottolivello, il menu comparirà a destra
                return {
                    position: 'absolute',
                    top: 0,
                    left: this.lunghezzaMassima(),
                    background: 'white',
                    display: 'block',
                    'list-style-type': 'none',
                    margin: 0,
                    padding: 0
                };
        },
        listaVisibile : function(){
console.log(' liv attuale: '+this.livelloMenu +'-> '+this.nome);
console.log(' liv mem: '+this.$store.getters.livelloMenu +'-> '+this.$store.getters.menuSelezionato);
console.log('');
            console.log('-->',this.$store.getters.sequenzaMenu);
            if(this.$store.getters.livelloMenu +1 == this.livelloMenu){
                console.log('a');
                return this.nome == this.$store.getters.menuSelezionato;
            }
            var oContext = this;
            return this.$store.getters.sequenzaMenu.find(function(sVoce){return sVoce == oContext.nome}) != null;
        }
    },
    mounted : function(){
        this.livelloMenu = this.livello()+1;
    },
    watch : {
        
    }

}
</script>

<style scoped>
#Menu{
    font-family:"Arial";
    font-size: 14px;
}
li{
    margin: 0;
    padding: 0;
    border: 1px solid gray !important;
}
</style>