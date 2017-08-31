<template>
  <div style="min-width:300px; max-width:300px; display:inline-block; vertical-align:top">
    <ul class="w3-ul w3-card-4">
      <li><h2><strong>Películas</strong></h2></li>
      <li style="overflow: hidden; text-overflow: ellipsis" class="w3-hover-blue" v-for="Pelicula in Peliculas" @click="peliculaSelected(Pelicula.Id)"> Título: {{Pelicula.Titulo}}</li>
    </ul>
  </div>
</template>

<script>
import EventBus from './event-bus.js'
import AppIcon from './App-icon.vue'
import appConfig from './config.js'

var httpURL = appConfig.URLPelicula;

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
          _this.Peliculas = data;
        })
        .fail(function(data) {
          let mensaje = 'No se pudo cargar la lista. Revise su conexión a Internet.';
          EventBus.$emit('showMessage', mensaje);
        });
      },

      peliculaSelected: function(id){
        EventBus.$emit('peliculaSelected', id);
      }
    },

    data: function () {
      return {
        Peliculas: [],
      }
    },

    mounted: function() {
      this.loadList();

      EventBus.$on('updateListPelicula', function() {
        this.loadList();
      }.bind(this));
    }
  }

  </script>
