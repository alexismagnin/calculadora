<script>
import Teclado from './Teclado.vue'
import Pantalla from './Pantalla.vue'

export default {
    data(){
        return{
            resultado: 0,
            operando1: null,
            operando2: null,
            operador: null,
            acumulado: '',
            digitos: '0',
            ultimoApretado: null
        }
    },
    
    components: {
        Teclado,
        Pantalla
    },

    methods: {
        calcular(){
            if (this.operando2 == null) {
                this.operando2 = this.operando1
            }
            if (this.ultimoApretado == '=') {
                this.operando1 = this.resultado
                this.textoHistorial = this.operando1 + this.operador + this.operando2 + '='    
            } else {
                this.textoHistorial += this.operando2 + '='
            }
            this.actualizarResultado()
            this.textoDigitos = this.resultado
            this.ultimoApretado = '='
        },
 
        resetear(){
            this.$refs.pantalla.resetPantalla()
            this.operando1 = null
            this.operando2 = null
            this.resultado = 0
            this.operador = null
            this.acumulado = ''
            this.ultimoApretado = null
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
                
                case 'x':
                    this.resultado = this.operando1 * this.operando2
                    break;
            }
        },

        numeroApretado(numero){
            if (this.ultimoApretado == '=') {
                this.resetear()
            }
            if (this.operador == null){
                if (this.operando1 == null || this.operando1 == '0') {
                    this.operando1 = numero
                } else {
                    this.operando1 += numero
                    }
                this.textoDigitos = this.operando1
            } else {
                if (this.operando2 == null || this.operando2 == '0'){
                    this.operando2 = numero
                } else {
                    this.operando2 += numero
                }
                this.textoDigitos = this.operando2
            }
            this.ultimoApretado = numero
        },

        operadorApretado(operador){
            if (this.ultimoApretado != '=') {
                if (this.operando2 != null){
                this.actualizarResultado()
                }
                this.ultimoApretado = operador
                this.operador = operador
                this.textoHistorial +=  this.operando1 + this.operador
            } else {
                this.textoHistorial = ''
                this.operando1 = this.resultado
                this.operando2 = null
                this.ultimoApretado = operador
                this.operadorApretado(operador)
            }
        }
    },

    computed: {
        textoDigitos: {
            set(texto){
                this.digitos = texto
                this.$refs.pantalla.textoDigitos = this.digitos
            },

            get(){
                return this.digitos
            }
        },

        textoHistorial: {
            set(texto) {
                this.acumulado = texto
                this.$refs.pantalla.textoAcumulado = this.acumulado
            },

            get(){
                return this.acumulado
            }
        }
    }
}
</script>

<template>
    <div class="calculadora">
        <Pantalla ref="pantalla"/>
        <Teclado 
            @calcular="calcular"
            @resetear="resetear"
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