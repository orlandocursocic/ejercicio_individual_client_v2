<template>
  <div id="app">
    <h1><strong>{{ title }}</strong></h1>
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
