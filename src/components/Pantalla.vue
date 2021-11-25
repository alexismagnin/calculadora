<script>
export default {
    data(){
        return {
            operando: '0',
            operador: null,
            acumulado: '',
        }
    },

    methods:{
        agregarTexto(cadena){
            if (cadena.length == 1 ) {
                if (!isNaN(cadena)){
                    if (this.operando == '0') {
                        this.operando = cadena
                    } else {
                        this.operando += cadena
                    }
                    if (this.operador != null){
                        this.operador = null
                    }
                } else {
                    if (cadena == '+' || cadena == '/' || cadena == '-' || cadena == '*'){
                        if (this.operador == null) {
                            this.operador = cadena
                            this.acumulado += (this.operando + this.operador)
                            this.operando = '0'
                        } else {
                            this.operador = cadena
                            this.acumulado = this.acumulado.substring(0, this.acumulado.length-1) + this.operador
                        }
                    }
                }
            } else {
                if (cadena.startsWith('=')){
                    this.acumulado = this.acumulado + this.operando + '='
                    this.operando = cadena.substring(1)
                } else {
                    console.warn('Argumento erróneo en pantalla')
                    throw new Error('Argumento erróneo en pantalla')
                }
            }
        }
    }
}
</script>

<template>
    <div class="pantalla" @mouseover="agregarTexto(auxiliar)">
        <span style="font-size: 15px">{{acumulado}}</span><br>
        <span style="font-size: 30px">{{operando}}</span>
    </div>
</template>

<style>
.pantalla {
    background-color: #999999;
    color: #FFFFFF;
    font-family: 'Courier New', Courier, monospace;
    text-align: right;
    border: 2px solid black;
    border-radius: 8px;
    padding: 4px;
    width: 184px;
    height: 48px;
    margin: 2px;
    overflow: hidden;
}
</style>