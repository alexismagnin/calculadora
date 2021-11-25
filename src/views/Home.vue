<template>
  <div class="home">
    <Pantalla />
    <Teclado />
  </div>
</template>

<script>
// @ is an alias to /src
import Teclado from '@/components/Teclado.vue'
import Pantalla from '@/components/Pantalla.vue'

export default {
  name: 'Home',
  components: {
    Teclado,
    Pantalla
  }
}
</script>
