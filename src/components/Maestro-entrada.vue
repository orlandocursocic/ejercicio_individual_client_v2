<template>
  <div style="display:inline-block; vertical-align:top">
    <ul class="w3-ul w3-card-4">
      <li><h2><strong>Entradas</strong></h2></li>
      <li class="w3-hover-blue" v-for="Entrada in Entradas" @click="entradaSelected(Entrada.Id)"> Precio: {{Entrada.Precio }}, Sala: {{ Entrada.Sala }} </li>
    </ul>
  </div>
</template>

<script>
import EventBus from './event-bus.js'
import AppIcon from './App-icon.vue'
import appConfig from './config.js'

var httpURL = appConfig.URLEntrada;

export default {
  components: {
    'app-icon' : AppIcon
  },

  methods: {

    loadList: function(){
      var _this = this;
      $.ajax(
        {
          url : httpURL,
          type: "GET",
        })
        .done(function(data) {
          _this.Entradas = data;
        })
        .fail(function(data) {
          let mensaje = 'No se pudo cargar la lista. Revise su conexión a Internet.';
          EventBus.$emit('showMessage', mensaje);
        });
      },

      entradaSelected: function(id){
        EventBus.$emit('entradaSelected', id);
      }
    },

    data: function () {
      return {
        Entradas: [],
      }
    },

    mounted: function() {
      this.loadList();

      EventBus.$on('updateListEntrada', function() {
        this.loadList();
      }.bind(this));
    }
  }

  </script>
