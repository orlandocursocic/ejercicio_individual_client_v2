<template>
  <div id="app">
    <h2>{{ title }}</h2>
    <maestro-entrada></maestro-entrada>
    <detalle-entrada></detalle-entrada>
  </div>
</template>

<script>
import MaestroEntrada from './Maestro-Entrada.vue'
import DetalleEntrada from './Detalle-Entrada.vue'

export default {
      components: {
          'maestro-entrada' : MaestroEntrada,
          'detalle-entrada' : DetalleEntrada
      },

      data: function() {
        return {
          title: 'Maestro-Detalle de Entrada',
        }
      }
}
</script>
