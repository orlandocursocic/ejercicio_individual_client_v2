<template>
  <div id="app">
    <master-chooser></master-chooser>
    <h2 v-show="opcion == 'entradas'"><strong>{{ title.Entradas }}</strong></h2>
    <maestro-entrada v-show="opcion == 'entradas'"></maestro-entrada>
    <detalle-entrada v-show="opcion == 'entradas'"></detalle-entrada>
    <h2 v-show="opcion == 'peliculas'"><strong>{{ title.Peliculas }}</strong></h2>
    <maestro-pelicula v-show="opcion == 'peliculas'"></maestro-pelicula>
    <detalle-pelicula v-show="opcion == 'peliculas'"></detalle-pelicula>
    <infomessage style="clear:both"></infomessage>
  </div>
</template>

<script>
import EventBus from './components/event-bus.js'
import Chooser from './components/Chooser.vue'
import MaestroEntrada from './components/Maestro-Entrada.vue'
import DetalleEntrada from './components/Detalle-Entrada.vue'
import MaestroPelicula from './components/Maestro-Pelicula.vue'
import DetallePelicula from './components/Detalle-Pelicula.vue'
import InfoMessage from './components/InfoMessage.vue'

export default {
  components: {
    'master-chooser' : Chooser,
    'maestro-entrada' : MaestroEntrada,
    'detalle-entrada' : DetalleEntrada,
    'maestro-pelicula' : MaestroPelicula,
    'detalle-pelicula' : DetallePelicula,
    'infomessage' : InfoMessage
  },

  data: function() {
    return {
      title: {
        Entradas: 'Maestro-Detalle de Entrada',
        Peliculas: 'Maestro-Detalle de Película'
      },
      opcion: 'entradas'
    }
  },

  mounted: function(){
    EventBus.$on('chooseOption', function(opcion) {
      this.opcion = opcion;
    }.bind(this));
  }
}
</script>
