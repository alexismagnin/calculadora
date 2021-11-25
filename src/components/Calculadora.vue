<script>
import Teclado from './Teclado.vue'
import Pantalla from './Pantalla.vue'

export default {
    data(){
        return{
            resultado: 0,
            operando1: null,
            operando2: null,
            operador: null
        }
    },
    
    components: {
        Teclado,
        Pantalla
    },

    methods: {
        calcular(){
            this.actualizarResultado()
            this.$refs.pantalla.agregarTexto('='+this.operando1)
        },

        actualizarResultado(){
            this.operando1 = parseInt(this.operando1)
            this.operando2 = parseInt(this.operando2)
            switch (this.operador) {
                case '+':
                    this.resultado = this.operando1 + this.operando2 
                    break;
                
                case '-':
                    this.resultado = this.operando1 - this.operando2
                    break;
                
                case '/':
                    this.resultado = this.operando1 / this.operando2
                    break;
                
                case '*':
                    this.resultado = this.operando1 * this.operando2
                    break;
            }
            this.operando2 = this.operando1
            this.operando1 = this.resultado
        },

        numeroApretado(numero){
            this.$refs.pantalla.agregarTexto(numero)
            if (this.operador == null){
                if (this.operando1 == null || this.operando1 == '0') {
                    this.operando1 = numero
                } else {
                    this.operando1 += numero
                    }
            } else {
                if (this.operando2 == null || this.operando2 == '0'){
                    this.operando2 = numero
                } else {
                    this.operando2 += numero
                }
            }
        },

        operadorApretado(operador){
            this.$refs.pantalla.agregarTexto(operador)
            if (this.operando2 != null){
                this.actualizarResultado()
            }
            this.operador = operador
        }
    }
}
</script>

<template>
    <div class="calculadora">
        <Pantalla ref="pantalla"/>
        <Teclado 
            @calcular="calcular"
            @numero-apretado="numeroApretado"
            @operador-apretado="operadorApretado" />
    </div>
</template>

<style>
.calculadora {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 230px;
    height: 300px;
    border-radius: 8px;
    background-color: lightgrey;
    margin: auto;
    padding: 5px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}
</style>