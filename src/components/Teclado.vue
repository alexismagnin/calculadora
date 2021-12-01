<script>
import Boton from "./Boton.vue"

export default {
    data() {
        return {

            }
    },

    emits: [
        'calcular',
        'resetear',
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

        resetear(){
            this.$emit('resetear')
        },

        numeroApretado(numero){
            this.$emit('numero-apretado',numero)
        },

        operadorApretado(operador){
            this.$emit('operador-apretado',operador)
        }
    }

}
</script>

<template>
    <div class="teclado">
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
        <Boton value=0 @click="numeroApretado" />
        <Boton value='C' spanX=1 @click="resetear" />
        <Boton value='=' spanX=1 @click="calcular" />
        <Boton value='/' @click="operadorApretado" />
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