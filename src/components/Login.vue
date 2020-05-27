<template>
    <form id="BoxLogin">
        <h2>LOGIN</h2>
        <table>
            <tr>
                <td><label for="utente">Utente: </label></td>
                <td><input id="utente" name="_utente_"></td>
            </tr>
            <tr>
                <td><label for="password">Password: </label></td>
                <td><input id="password" name="_p4ssw0rd_" type="password" v-model="utente.password"></td>
            </tr>
            <tr>
                <td></td><td><input value="Accedi" type="submit"></td>
            </tr>
            <tr  class="info" >
                <td colspan="2"><a ref="">Nome utente dimenticato?</a></td>
            </tr>
            <tr  class="info" >
                <td colspan="2"><a ref="#">Password dimenticata?</a></td>
            </tr>
        </table>
        
        
    </form>
</template>

<script>
export default {
     name: 'Login',
     data : function(){
         return{
             utente : {
                 nome : '',
                 password : ''
             }
         }
     }
}
</script>

<style scoped>
#BoxLogin{
    font-family:Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
    border: 1px solid #222;
    margin: 1em;
    padding: 0.5em;
    width: 20em;
}
h2{
    text-align: center;
    margin-top: 0;
}
label{
    color: #333;
    font-weight: bolder;
}
td{
    width: 50%;
}
tr{
    height: 2em;
}
tr.info{
   height: 1em;
}
a{
    font-size: 0.7em;
}
a:hover{
    color: blue;
    cursor: pointer;
}
</style>