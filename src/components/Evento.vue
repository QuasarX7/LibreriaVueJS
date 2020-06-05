<template>
    <div>
        <h1>{{nome}}</h1>
        <form>
            <table>
                <tr>
                    <td>
                        Tipo:
                    </td>
                    <td colspan="2">
                        <select name="tipo" id="tipo" style="width:100%">
                            <option>---seleziona---</option>
                        </select>
                    </td>
                </tr>
                <tr>
                    <td>
                        Inizio:
                    </td>
                    <td>
                        <input name="data_inizio" type="date"/>
                    </td>
                    <td>
                        <input name="ora_inizio" type="time"/>
                    </td>
                </tr>
                <tr>
                    <td>
                        Fine:
                    </td>
                    <td>
                        <input name="data_fine" type="date"/>
                    </td>
                    <td>
                        <input name="ora_fine" type="time"/>
                    </td>
                </tr>
                <tr><td colspan="2">Note:</td></tr>
                <tr>
                    <td colspan="3">
                        <textarea cols="35" rows="10"/>
                    </td>
                </tr>
            </table>
        </form>
    </div>
</template>

<script>
export default {
    name : 'Evento',
    props : ['nome'],
    data : function(){
        return {

        }
    },
    methods : function(){

    },
    mounted : function(){

    }
}
</script>


<style scoped>

</style>

