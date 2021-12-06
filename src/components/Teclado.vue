<script>
import Boton from "./Boton.vue"

export default {
    data() {
        return {
            activarMemos: false
            }
    },

    emits: [
        'calcular',
        'resetear',
        'numero-apretado',
        'operador-apretado',
        'memorizar',
        'traer-memo',
        'borrar-memo',
        'cambiar-signo'
    ],

    components: {
        Boton
        },

    methods:{
        calcular(){
            this.$emit('calcular')
        },

        resetear(){
            this.$emit('resetear')
        },

        numeroApretado(numero){
            this.$emit('numero-apretado',numero)
        },

        operadorApretado(operador){
            this.$emit('operador-apretado',operador)
        },

        memorizar(){
            this.$emit('memorizar')
        },

        traerMemo(){
            this.$emit('traer-memo')
        },

        borrarMemo(){
            this.$emit('borrar-memo')
        },

        cambiarSigno(){
            this.$emit('cambiar-signo')
        }
    }

}
</script>

<template>
    <div class="teclado">
        <Boton value='M' @click="memorizar" />
        <Boton value='MR' @click="traerMemo" :disabled="!activarMemos" />
        <Boton value='MC' @click="borrarMemo" :disabled="!activarMemos" />
        <Boton value='C' @click="resetear" />
        <Boton value=1 @click="numeroApretado" />
        <Boton value=2 @click="numeroApretado" />
        <Boton value=3 @click="numeroApretado" />
        <Boton value='+' @click="operadorApretado" />
        <Boton value=4 @click="numeroApretado" />
        <Boton value=5 @click="numeroApretado" />
        <Boton value=6 @click="numeroApretado" />
        <Boton value='-' @click="operadorApretado" />
        <Boton value=7 @click="numeroApretado" />
        <Boton value=8 @click="numeroApretado" />
        <Boton value=9 @click="numeroApretado" />
        <Boton value='x' @click="operadorApretado" />
        <Boton value=. @click="numeroApretado" />
        <Boton value=0 @click="numeroApretado" />
        <Boton value='±' @click="cambiarSigno" />
        <Boton value='/' @click="operadorApretado" />
        <Boton value='=' spanX=4 @click="calcular" />
    </div>
</template>

<style>
.teclado {
    display: flex;
    width: 200px;
    height: auto;
    flex-wrap: wrap;
    justify-content: center;
}
</style>