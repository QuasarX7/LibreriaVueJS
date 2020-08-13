<template>
<div id="box">
    <div id="controllo">
        <table>
            <tr>
                <td></td>
                <td><button @click="muovi('su')">▲</button></td>
                <td></td>
            </tr>
            <tr>
                <td><button  @click="muovi('sinistra')">◄</button></td>
                <td>○</td>
                 <td><button @click="muovi('destra')">►</button></td>
            </tr>
            <tr>
                <td></td>
                 <td><button @click="muovi('giu')">▼</button></td>
                <td></td>
            </tr>
        </table>
        <div id="zoom">
            <button @click="zoom">+</button>
            <button @click="estendi">-</button>
        </div>
    </div>
    <canvas ref="grafico"  @contextmenu.prevent="muovi('sinistra')" @click.left="zoom" >

    </canvas>
</div>
</template>

<script>
export default {
    name : 'DiagrammaXY',
    props : ['lunghezza','altezza','unita','funzione'],
    data : function(){
        return {
            matita : null,
            scala : 1,
            spostamentoX : 0,
            spostamentoY : 0,
        }
    },
    methods : {
        disegna : function () {
            let unita = Number(this.unita*this.scala);
            let lunghezza = Number(this.$refs.grafico.width);
            let altezza = Number(this.$refs.grafico.height);
            
            this.matita = this.$refs.grafico.getContext('2d');
            this.matita.clearRect(0, 0, lunghezza, altezza); // cancella al rendering
            
            this.griglia(lunghezza,altezza,unita);
            this.asseX();
            this.asseY();
            this.grafico(lunghezza,altezza,unita);
            this.matita.stroke();
        },
        retta : function (x,y,x2,y2) {
            this.matita.beginPath();
            this.matita.strokeStyle = 'gray';
            this.matita.lineWidth = 0.40;
            this.matita.moveTo(x,y);
            this.matita.lineTo(x2,y2);
            this.matita.closePath();
            this.matita.stroke();
        },
        indice : function (valore,x,y,x2,y2) {
            this.matita.beginPath();
            this.matita.strokeStyle = 'black';
            let dx = this.spostamentoX*this.unita*this.scala;
            let dy = this.spostamentoY*this.unita*this.scala;
            this.matita.strokeText(valore,(x+x2)/2+5-dx, (y+y2)/2-5-dy);
            this.matita.closePath();
            this.matita.stroke();
        },
        punto : function (x,y,x0,y0,unita) {
            var delta = 1;
            this.matita.moveTo(x*unita+x0,y0-y*unita);
            this.matita.lineTo(x*unita+x0+delta,y0-y*unita+delta);
        },
        griglia : function(lunghezza,altezza,unita){
            this.matita.beginPath();
            //ascisse
            for(let x=0; x < lunghezza; x += unita){
                this.retta(x+lunghezza/2, 0, x+lunghezza/2, altezza);
                this.indice(x/unita+'',x+lunghezza/2, 0, x+lunghezza/2, altezza);
                this.retta(-x+lunghezza/2, 0, -x+lunghezza/2, altezza);
                this.indice(-x/unita+'',-x+lunghezza/2, 0, -x+lunghezza/2, altezza);
            }
            // ordinate
            for(let y=0; y < altezza; y += unita){
                this.retta(0,y+altezza/2,lunghezza,y+altezza/2);
                this.indice(y/unita+'',0,y+altezza/2,lunghezza,y+altezza/2);
                this.retta(0,-y+altezza/2,lunghezza,-y+altezza/2);
                this.indice(-y/unita+'',0,-y+altezza/2,lunghezza,-y+altezza/2);
            }
            this.matita.closePath();
            this.matita.stroke();
        },
        asseX : function(){
            this.matita.beginPath();
            this.matita.strokeStyle = 'black';
            this.matita.lineWidth = 2;
            let dy = this.spostamentoY*this.unita*this.scala;
            this.matita.moveTo( 0,               this.altezza/2 - dy);
            this.matita.lineTo( this.lunghezza,  this.altezza/2 - dy);
            this.matita.closePath();
            this.matita.stroke();
        },
        asseY : function(){
            this.matita.beginPath();
            this.matita.strokeStyle = 'black';
            this.matita.lineWidth = 2;
            let dx = this.spostamentoX*this.unita*this.scala;
            this.matita.moveTo( this.lunghezza/2 - dx,  0 );
            this.matita.lineTo( this.lunghezza/2 - dx,  this.altezza );
            this.matita.closePath();
            this.matita.stroke();
        },
        grafico : function(lunghezza,altezza,unita){
            let funzioneX = this.funzione;
            if(this.spostamentoX){
                // es.: x*x-2*x  -> (x+1)*(x+1)-2*(x+1)
                funzioneX = funzioneX.replace(new RegExp('x', 'g'), '(x+'+this.spostamentoX+')')
            }
            if(this.spostamentoY){
                // es.: x*x-2*x  -> es.: x*x-2*x + 5
                funzioneX += " + " + this.spostamentoY;
            }
            this.matita.strokeStyle = 'red';
            this.matita.lineWidth = 2;
            this.matita.beginPath();
            for(let x=-lunghezza/2; x < lunghezza/2; x += 1/unita){
                
               this.punto(x,eval(funzioneX),lunghezza/2,altezza/2,unita);
            }
            
            this.matita.closePath();
            this.matita.stroke();
        },
        zoom : function(){
            this.scala *= 2;
            this.disegna();
            
        },
        estendi :function(){
             this.scala *= 0.5;
             this.disegna();
        },
        muovi: function(direzione){
            if(direzione == 'sinistra')
                this.spostamentoX += 1;
            else if(direzione == 'destra')
                this.spostamentoX -= 1;
            else if(direzione == 'su')
                this.spostamentoY += 1;
            else if(direzione == 'giu')
                this.spostamentoY -= 1;
            this.disegna();
        }
    },
    mounted : function () {
        this.$refs.grafico.width = this.lunghezza;
        this.$refs.grafico.height = this.altezza;
        this.disegna();
    }
}
</script>

<style scoped>
canvas{
    background-color:white;
    border: 1px solid black;
}
button{
    width: 1.5em;
    height:1.5em;
    font-size: 1em;
    padding: 0;
}
table{
    position:absolute;
    top: 0;
    left: 0;
    border-collapse: collapse;
}
table td{
   margin: 0;
   padding:0;
}
#box{
    position: relative;
}
#controllo{
    position:absolute;
    top:0;
    left:0;
}
#zoom{
    position:absolute;
    top: 0;
    left: 5em;
    width: 3em;
}

</style>