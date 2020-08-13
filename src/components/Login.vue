<template>
    <form id="BoxLogin" method="POST" :action="hostServer">
        <template  v-if="infoPassword === false && infoUtente === false">
            <h2>LOGIN</h2>
            <table>
                <tbody>
                    <tr>
                        <td><label for="utente">Utente: </label></td>
                        <td><input id="utente" name="_utente_" v-model="utente.nome"></td>
                    </tr>
                    <tr>
                        <td><label for="password">Password: </label></td>
                        <td><input id="password" name="_p4ssw0rd_" type="password" v-model="utente.password"></td>
                    </tr>
                    <tr>
                        <td></td><td><input value="Accedi" type="submit"></td>
                    </tr>
                </tbody>
                <tfoot>
                    <tr class="info" >
                        <td colspan="2"><a @click="richiestaInfo('utente')" >Nome utente dimenticato?</a></td>
                    </tr>
                    <tr  class="info" >
                        <td colspan="2"><a @click="richiestaInfo('password')">Password dimenticata?</a></td>
                    </tr>
                </tfoot>
            </table>
        </template>
        <template v-else>
            <input type="hidden" name="info" :value="utente.aiuto" >
            <h2>Richiesta di aiuto</h2>
            <table>
                <tbody>
                    <tr>
                        <td><label for="email">Propria E-mail: </label></td>
                        <td><input id="email" name="email" v-model.trim="utente.email"></td>
                    </tr>
                    <tr>
                        <td></td><td><input :value="utente.aiuto" type="submit"></td>
                    </tr>
                </tbody>
            </table>
        </template>
    </form>
</template>

<script>
export default {
     name: 'Login',
     props : ['hostServer'],
     data : function(){
         return{
             utente : {
                 nome : '',
                 password : '',
                 email : '',
                 aiuto : ''
             },
            infoPassword : false,
            infoUtente : false
         }
     },
     methods : {
         richiestaInfo : function(tipo){
             if(tipo == 'utente'){
                 this.utente.aiuto = "Richiesta info nome " + tipo;
                 this.infoUtente = true;
             }else if(tipo == 'password'){
                 this.utente.aiuto = "Richiesta cambio " + tipo;
                 this.infoPassword = true;
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