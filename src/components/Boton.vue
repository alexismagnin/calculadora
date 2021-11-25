<script>
export default { 
    props: {
        value: {
            type: String,
            required: true
        },
        spanX: {
            default: 1,
            validator: value => {
                const valor = parseInt(value)
                if (!Number.isInteger(valor) || valor <= 0){
                    console.warn('El valor de spanX debe ser un entero mayor que 0')
                    return false
                } else {
                    return true
                }
            }
        },
        spanY: {
            default: 1,
            validator: value => {
                const valor = parseInt(value)
                if (!Number.isInteger(valor) || valor <= 0){
                    console.warn('El valor de spanY debe ser un entero mayor que 0')
                    return false
                } else {
                    return true
                }
            }
        }
    },

    emits: ['click'],

    computed:{
        dimensiones(){
            return {
                ancho: 45 * parseInt(this.spanX) + 2 * (parseInt(this.spanX)-1),
                alto: 45 * parseInt(this.spanY) + 2 * (parseInt(this.spanY)-1)
            }
        }
    },

    methods: {
        onClick() {
            this.$emit('click',this.value)
        }
    },

    beforeMount(){
        if (parseInt(this.spanX) > 1) this.spanY = 1
    }
}
</script>

<template>
    <button 
    class="botones"
    :style="{ width: dimensiones.ancho + 'px', height: dimensiones.alto + 'px' }"
    @click="onClick">{{ value }}</button>
</template>

<style>
.botones {
    font-size: 18px;
    font-weight: bold;
    font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
    margin: 2px;
    background-color: lightgray;
    border: 2px solid steelblue;
    border-radius: 8px;
    color: steelblue;
    text-align: center;
    cursor: pointer;
    transition-duration: 0.2s;
    box-shadow: 2px 2px 4px 0 rgba(0, 0, 0, 0.2)
}
.botones:hover {
    background-color: steelblue;
    color: white
}
.botones:disabled{
    background-color: lightsteelblue;
    color: white;
    border-color: lightsteelblue;
    cursor: default;
}
</style>