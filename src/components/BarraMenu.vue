<template>
    <div id="BarraMenu" :style="stile">
        <ul>
            <li><span><slot></slot></span></li>
        </ul>
    </div>
</template>

<script>
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex);

const MemoriaMenu = {
    AGGIUNGI_MENU_SELEZZIONATO : "aggiungiNomeMenu",
    LIVELLO_MENU_SELEZZIONATO : "livelloMenuCorrente"
};

const store = new Vuex.Store({
    state : {
        menu : []
    },
    getters : {
        menuSelezionato: function(state){
            if(state.menu.length > 0)
                return state.menu[state.menu.length-1];
        },
        livelloMenu: function(state){
            return state.menu.length;
        },
        sequenzaMenu: function(state){
            return state.menu;
        },

    },
    mutations : {
        [MemoriaMenu.AGGIUNGI_MENU_SELEZZIONATO] : function(state,menu){
            state.menu.push(menu);
        },
        [MemoriaMenu.LIVELLO_MENU_SELEZZIONATO] : function(state,livello){
            while(livello >= 0 && livello < state.menu.length){
                state.menu.pop();
            }
        }
    },
    actions : {
        aggiungiMenuCorrente: function(context, oMenu){
            context.commit(MemoriaMenu.LIVELLO_MENU_SELEZZIONATO, oMenu.livello);
            context.commit(MemoriaMenu.AGGIUNGI_MENU_SELEZZIONATO, oMenu.menu);
        }
    }
});

export default {
    name : 'BarraMenu',
    props : ['colore','sfondo'],
    data : function(){
        return {}
    },
    store,
    computed : {
        stile : function(){
            return {
                '--color' : this.colore,
                '--background' : this.sfondo,
                '--color-hover' : this.sfondo,
                '--background-hover' : this.colore
            };
        }
    },
    mounted : function(){
    }

}
</script>

<style scoped>
#BarraMenu{
    font-family:"Arial";
    font-size: 14px;
    width: 100%;
}

div{
    color : var(--color);
    background: var(--background);
}
div:hover{
    color : var(--color-hover);
    background: var(--background-hover);
}

ul{
    list-style-type: none;
    margin: 0;
    padding: 0;
}

li div, li div div{
    display: inline-block !important;
    border: 1px solid gray;
}
</style>