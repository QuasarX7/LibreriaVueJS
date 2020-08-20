<template>
    <div id="BarraMenu">
        <ul>
            <li><span><slot></slot></span></li>
        </ul>
    </div>
</template>

<script>
export default {
    name : 'BarraMenu',
    props : [],
    data : function(){
        return {}
    },
}
</script>

<style scoped>
#BarraMenu{
    font-family:"Arial";
    font-size: 14px;
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