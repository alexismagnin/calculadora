<script>
import Teclado from './Teclado.vue'
import Pantalla from './Pantalla.vue'

export default {
    data(){
        return{
            resultado: 0,
            operando1: 0,
            operando2: null,
            operador: null,
            acumulado: '',
            digitos: '0',
            ultimoApretado: null,
            memoria: null,
            botonesMemos: false,
            resetearCuenta: false
        }
    },
    
    components: {
        Teclado,
        Pantalla
    },

    methods: {
        calcular(){
            if (this.operador == null){
                this.textoHistorial = this.operando1 + '='
            } else {
                if (this.operando2 == null) {
                    this.operando2 = this.operando1
                }
                if (this.ultimoApretado == '=' || (this.ultimoApretado == 'M' && this.resetearCuenta)) {
                    this.operando1 = this.resultado
                    this.textoHistorial = this.operando1 + this.operador + this.operando2 + '='
                    this.resetearCuenta = true 
                } else {
                    this.textoHistorial += this.operando2 + '='
                }
            }
            this.actualizarResultado()
            this.textoDigitos = this.resultado
            this.ultimoApretado = '='
            this.resetearCuenta = true
        },
 
        resetear(){
            this.$refs.pantalla.resetPantalla()
            this.operando1 = null
            this.operando2 = null
            this.resultado = 0
            this.operador = null
            this.acumulado = ''
            this.ultimoApretado = null
            console.log("reset");
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
                
                default:
                    this.resultado = this.operando1
                    console.log("default");
            }
        },

        numeroApretado(numero){
            if (this.resetearCuenta) {
                this.resetearCuenta ? this.resetearCuenta = false : {}
                this.resetear()
            }
            if (this.operador == null){
                if (this.operando1 == null || this.operando1 == '0' || this.ultimoApretado == 'M') {
                    this.operando1 = numero
                } else {
                    this.operando1 += numero
                    }
                this.textoDigitos = this.operando1
            } else {
                if (this.operando2 == null || this.operando2 == '0' || this.ultimoApretado == 'M'){
                    this.operando2 = numero
                } else {
                    this.operando2 += numero
                }
                this.textoDigitos = this.operando2
            }
            this.ultimoApretado = numero
        },

        operadorApretado(operador){
            if (this.ultimoApretado != '=' && !this.resetearCuenta) {
                if (this.operando2 == null){
                    this.operador = operador
                    this.textoHistorial = this.operando1 + this.operador
                } else {
                    this.actualizarResultado()
                    this.operador = operador
                    this.textoHistorial +=  this.operando2 + this.operador
                    this.operando1 = this.resultado
                    this.operando2 = 0
                    this.textoDigitos = this.operando2
                    }
                this.ultimoApretado = operador
            } else {
                this.textoHistorial = ''
                this.operando1 = this.resultado
                this.operando2 = null
                this.ultimoApretado = operador
                this.resetearCuenta ? this.resetearCuenta = false : {}
                this.operadorApretado(operador)
            }
        },

        memorizar(){
            if (this.ultimoApretado != 'M'){
                if (this.operador == null){
                this.memoria = this.operando1
                } else {
                    if (this.ultimoApretado == '='){
                        this.memoria = this.resultado
                        this.resetearCuenta = true
                    } else {
                        this.memoria = this.operando2
                    }
                }
                this.ultimoApretado = 'M'
                this.activarMemos = true
            }
        },

        traerMemoria(){
            if (this.operador == null || this.resetearCuenta){
                this.operando1 = null
            } else {
                this.operando2 = null
            }
            this.numeroApretado(this.memoria)
        },

        borrarMemoria(){
            this.memoria = null
            this.activarMemos = false
        },

        cambiarSigno(){
            if (this.resetearCuenta) {
                this.numeroApretado(this.resultado * -1)
            } else {
                if (this.operador == null){
                    if (this.operando1 != null) {
                        this.operando1 *= -1
                        this.textoDigitos = this.operando1
                    }
                } else {
                    if (this.operando2 != null){
                        this.operando2 *= -1
                        this.textoDigitos = this.operando2
                }                 
            }
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
        },

        activarMemos: {
            set(valor){
                this.botonesMemos = valor
                this.$refs.teclado.activarMemos = this.botonesMemos
            },

            get(){
                return this.botonesMemos
            }
        }
    }
}
</script>

<template>
    <div class="calculadora">
        <Pantalla ref="pantalla"/>
        <Teclado  ref="teclado"
            @calcular="calcular"
            @resetear="resetear"
            @memorizar="memorizar"
            @traer-memo="traerMemoria"
            @borrar-memo="borrarMemoria"
            @cambiar-signo="cambiarSigno"
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
    box-sizing: content-box;
    width: fit-content;
    height: fit-content;
    border-radius: 8px;
    background-color: lightgrey;
    margin: auto;
    padding: 20px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}
</style>