<template>
    <canvas ref="grafico">

    </canvas>
</template>

<script>
export default {
    name : 'DiagrammaXY',
    props : ['lunghezza','altezza','unita','funzione'],
    data : function(){
        return {
            matita : null
        }
    },
    methods : {
        disegna : function () {
            let unita = Number(this.unita);
            let lunghezza = Number(this.$refs.grafico.width);
            let altezza = Number(this.$refs.grafico.height);
            
            this.matita = this.$refs.grafico.getContext('2d');
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
            this.matita.strokeText(valore,(x+x2)/2+5, (y+y2)/2-5);
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
            this.matita.moveTo( 0,               this.altezza/2 );
            this.matita.lineTo( this.lunghezza,  this.altezza/2 );
            this.matita.closePath();
            this.matita.stroke();
        },
        asseY : function(){
            this.matita.beginPath();
            this.matita.strokeStyle = 'black';
            this.matita.lineWidth = 2;
            this.matita.moveTo( this.lunghezza/2,  0 );
            this.matita.lineTo( this.lunghezza/2,  this.altezza );
            this.matita.closePath();
            this.matita.stroke();
        },
        grafico : function(lunghezza,altezza,unita){
            this.matita.strokeStyle = 'red';
            this.matita.lineWidth = 2;
            this.matita.beginPath();
            for(let x=-lunghezza/2; x < lunghezza/2; x += 1/unita){
                this.punto(x,eval(this.funzione),lunghezza/2,altezza/2,unita);
            }
            
            this.matita.closePath();
            this.matita.stroke();
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

</style>