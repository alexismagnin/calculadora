<script>
import Boton from "./Boton.vue"

export default {
    data() {
        return {
            operando1: null,
            operando2: null,
            operador: null
            }
    },

    emits: [
        'calcular',
        'numero-apretado',
        'operador-apretado'
    ],

    components: {
        Boton
        },

    methods:{
        calcular(){
            this.$emit('calcular')
        },

        numeroApretado(numero){
            if (this.operador == null){
                if (this.operando1 == null) {
                    this.operando1 = numero
                } else {
                    this.operando1 += numero
                    }
            } else {
                if (this.operando2 == null){
                    this.operando2 = numero
                } else {
                    this.operando2 += numero
                }
            }
            this.$emit('numero-apretado',numero)
        },

        operadorApretado(operador){
            if (operador == '='){
                this.calcular()
            } else {
                this.operador = operador
                this.$emit('operador-apretado',operador)
            }

        }
    }

}
</script>

<template>
    <div class="teclado">
        <Boton value=1 @click="numeroApretado" />
        <Boton value=2 @click="numeroApretado" />
        <Boton value=3 @click="numeroApretado" />
        <Boton value='+' @click="operadorApretado" :disabled="this.operando1 == null" />
        <Boton value=4 @click="numeroApretado" />
        <Boton value=5 @click="numeroApretado" />
        <Boton value=6 @click="numeroApretado" />
        <Boton value='-' @click="operadorApretado" :disabled="this.operando1 == null"/>
        <Boton value=7 @click="numeroApretado" />
        <Boton value=8 @click="numeroApretado" />
        <Boton value=9 @click="numeroApretado" />
        <Boton value='*' @click="operadorApretado" :disabled="this.operando1 == null"/>
        <Boton value=0 @click="numeroApretado" />
        <Boton value='=' spanX=2 @click="operadorApretado" :disabled="this.operando2 == null"/>
        <Boton value='/' @click="operadorApretado" :disabled="this.operando1 == null"/>
    </div>
</template>

<style>
.teclado {
    display: flex;
    width: 200px;
    height: 200px;
    flex-wrap: wrap;
    justify-content: center;
}
</style>