<template>
    <div id="VoceMenu" :style="stile">
        <a :href="collegamento">{{nome}}</a>
    </div>
</template>

<script>
export default {
    name : 'VoceMenu',
    props : ['nome','link','colore','sfondo'],
    data : function(){
        return {
            collegamento : ''
        }
    },
    methods : {

    },
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
    created :function(){
        if(this.link){
            this.collegamento = this.link;
        }else{
            this.collegamento = ''
        }
    }
}
</script>

<style scoped>
#VoceMenu{
    display: block;
    width: '100%';
    white-space: nowrap;
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

a{
    display: inline-block;
    outline: none;
    text-decoration: none;
    padding:0.4em;
    margin: 0;
}
a:link, a:visited {
  color: inherit; /*eredita*/
}
</style>