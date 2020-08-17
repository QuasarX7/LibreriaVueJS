<template>
    <div id="Menu" ref="menu" :style="stileEtichetta">
        <div :id="nome" style="height:100%" @click="eventoClic">{{nome}}</div>
        <template v-if="listaVisibile">
            <ul :style="stile">
                <li><slot></slot></li>
            </ul>
        </template>
        <canvas ref="canvas" width="0" height="0" style="display:none"></canvas>
    </div>
</template>

<script>
export default {
    name : 'Menu',
    props : ['nome'],
    data : function(){
        return {
            listaVisibile : false
        }
    },
    
    methods : {
        eventoClic : function(e){
            if(e.target.id === this.nome){ 
                // la commutazione avviene solo per l'elemento "Menu" che lo ha generato
                this.listaVisibile = !this.listaVisibile;
            }
        },
         getTextWidth : function (text, font) {
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
                        if(max < this.getTextWidth(link.textContent,"12px Arial"))
                            max = this.getTextWidth(link.textContent,"12px Arial"); // lunghezza stringa voce
                        riga = riga.nextSibling;
                    }
                    return (max/12.0 + 0.77) + 'em';
                }
            }
            return '100%';
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
            if(this.$refs.menu.parentElement.tagName != "LI")
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
        }
    },
    mounted : function(){
    }

}
</script>

<style scoped>
ul{
    font-family:"Arial";
    font-size: 14px;
}
li{
    margin: 0;
    padding: 0;
    border: 1px solid gray;
}
</style>