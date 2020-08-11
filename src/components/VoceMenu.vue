<template>
    <div id="VoceMenu">
        <a :href="collegamento">{{nome}}</a>
    </div>
</template>

<script>
export default {
    name : 'VoceMenu',
    props : ['nome','link'],
    data : function(){
        return {
            collegamento : ''
        }
    },
    methods : {

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
    padding:0;
    margin: 0;
    border-top: 1px solid gray;
}
a{
    display: inline-block;
    outline: none;
    text-decoration: none;
    padding:0.4em;
    margin: 0;
}
a:link, a:visited {
  color: black;
}
</style>