<template>
  <div class="w3-container w3-card-4" style="min-width:400px; max-width:400px; display:inline-block; vertical-align:top">
    <div>
      <h3 style="overflow: hidden; text-overflow: ellipsis; max-width:400px"><strong>Película: </strong>{{Pelicula.Titulo}}</h3>
      <label class="w3-text" for="titulo"> Título </label>
      <input class="w3-input w3-border" style="overflow: hidden; text-overflow: ellipsis" type="string" name="titulo" value="Titulo" :disabled="!editing && !addingNew" v-model="Pelicula.Titulo">
      <label class="w3-text" for="anyo"> Año </label>
      <input class="w3-input w3-border" style="overflow: hidden; text-overflow: ellipsis" type="numeric" name="anyo" value="Año" :disabled="!editing && !addingNew" v-model="Pelicula.Anyo">
      <label class="w3-text" for="director"> Director </label>
      <input class="w3-input w3-border" style="overflow: hidden; text-overflow: ellipsis" type="string" name="director" value="Director" :disabled="!editing && !addingNew" v-model="Pelicula.Director">
      <label class="w3-text" for="pais"> País </label>
      <input class="w3-input w3-border" style="overflow: hidden; text-overflow: ellipsis" type="string" name="pais" value="Pais" :disabled="!editing && !addingNew" v-model="Pelicula.Pais">
    </div>

    <br>

    <div>
      <template v-if="!addingNew">
        <div style="float: left">
          <button type="button" class="btn btn-default btn-sm" title="Nuevo" @click="editNew" :disabled="editing">
            <app-icon img="plus"></app-icon>
          </button>
        </div>
      </template>

      <template v-else>
        <div style="float: left">
          <button type="button" class="btn btn-default btn-sm" title="Confirmar" @click="validateNew">
            <app-icon img="ok"></app-icon>
          </button>
          <button type="button" class="btn btn-default btn-sm" title="Descartar" @click="discardNew">
            <app-icon img="remove"></app-icon>
          </button>
        </div>
      </template>

      <template v-if="!editing">
        <div style="float: right">
          <button type="button" class="btn btn-default btn-sm" title="Editar" @click="validateIdUpdate" :disabled="addingNew">
            <app-icon img="edit"></app-icon>
          </button>
          <button type="button" class="btn btn-default btn-sm" title="Eliminar" @click="validateIdDelete" :disabled="addingNew">
            <app-icon img="trash"></app-icon>
          </button>
        </div>
      </template>

      <template v-else>
        <div style="float: right">
          <button type="button" class="btn btn-default btn-sm" title="Confirmar" @click="validateUpdate">
            <app-icon img="ok"></app-icon>
          </button>
          <button type="button" class="btn btn-default btn-sm" title="Descartar" @click="discard">
            <app-icon img="remove"></app-icon>
          </button>
        </div>
      </template>
    </div>

    <div>
      <p style="visibility: hidden">separador</p>
    </div>
    <br>
  </div>
</template>

<script>
import EventBus from './event-bus.js'
import AppIcon from './App-icon.vue'
import appConfig from './config.js'
import InfoMessage from './InfoMessage.vue'

var httpURL = appConfig.URLPelicula;
var maxInt =  2147483647;

export default {
  components: {
    'app-icon' : AppIcon,
    'infomessage' : InfoMessage
  },

  methods: {
    validateNew: function() {
      let mensaje ='';
      if(this.Pelicula.Titulo == '') {
        mensaje = 'El Título de la película no puede estar vacío.';
        EventBus.$emit('showMessage', mensaje);
      } else if(!this.isInt(this.Pelicula.Anyo) || this.Pelicula.Anyo <= 1800) {
        mensaje = 'El año de la película debe ser un número entero mayor que 1800.';
        EventBus.$emit('showMessage', mensaje);
      } else if(this.isNumeric(this.Pelicula.Director) || this.Pelicula.Director == '') {
        mensaje = 'El nombre del director no puede ser un número ni estar vacío.';
        EventBus.$emit('showMessage', mensaje);
      } else if(this.isNumeric(this.Pelicula.Pais) || this.Pelicula.Pais == '') {
        mensaje = 'El país de la película no puede ser un número ni estar vacío';
        EventBus.$emit('showMessage', mensaje);
      } else if(this.Pelicula.anyo > maxInt) {
        mensaje = 'El año debe ser un número menor que ' + maxInt;
        EventBus.$emit('showMessage', mensaje);
      } else {
        this.create();
      }
    },
    validateIdUpdate: function() {
      let mensaje ='';
      if(this.Pelicula.Id =='' || this.Pelicula.Id < 0) {
        mensaje = 'Seleccione una película de la lista.';
        EventBus.$emit('showMessage', mensaje);
      } else {
        this.edit();
      }
    },
    validateIdDelete: function() {
      let mensaje ='';
      if(this.Pelicula.Id =='' || this.Pelicula.Id < 0) {
        mensaje = 'Seleccione una película de la lista.';
        EventBus.$emit('showMessage', mensaje);
      } else {
        this.remove();
      }
    },
    validateUpdate: function() {
      let mensaje ='';
      if(this.Pelicula.Titulo == '') {
        mensaje = 'El Título de la película no puede estar vacío.';
        EventBus.$emit('showMessage', mensaje);
      } else if(!this.isInt(this.Pelicula.Anyo) || this.Pelicula.Anyo <= 1800) {
        mensaje = 'El año de la película debe ser un número entero mayor que 1800.';
        EventBus.$emit('showMessage', mensaje);
      } else if(this.isNumeric(this.Pelicula.Director) || this.Pelicula.Director == '') {
        mensaje = 'El nombre del director no puede ser un número ni estar vacío.';
        EventBus.$emit('showMessage', mensaje);
      } else if(this.isNumeric(this.Pelicula.Pais) || this.Pelicula.Pais == '') {
        mensaje = 'El país de la película no puede ser un número ni estar vacío';
        EventBus.$emit('showMessage', mensaje);
      } else if(this.Pelicula.Anyo > maxInt) {
        mensaje = 'El año debe ser un número menor que ' + maxInt;
        EventBus.$emit('showMessage', mensaje);
      } else {
        this.update();
      }
    },
    isNumeric: function(n) {
      return !isNaN(parseFloat(n)) && isFinite(n);
    },
    isInt: function(n) {
      return n % 1 === 0;
    },
    editNew: function () {
      this.PeliculaCopia.Titulo = this.Pelicula.Titulo;
      this.PeliculaCopia.Anyo = this.Pelicula.Anyo;
      this.PeliculaCopia.Director = this.Pelicula.Director;
      this.PeliculaCopia.Pais = this.Pelicula.Pais;

      this.Pelicula.Titulo = '';
      this.Pelicula.Anyo = '';
      this.Pelicula.Director = '';
      this.Pelicula.Pais = '';
      this.addingNew = true;
    },
    discardNew: function () {
      this.Pelicula.Titulo = this.PeliculaCopia.Titulo;
      this.Pelicula.Anyo = this.PeliculaCopia.Anyo;
      this.Pelicula.Director = this.PeliculaCopia.Director;
      this.Pelicula.Pais = this.PeliculaCopia.Pais;
      this.addingNew = false;
    },
    edit: function () {
      this.PeliculaCopia.Titulo = this.Pelicula.Titulo;
      this.PeliculaCopia.Anyo = this.Pelicula.Anyo;
      this.PeliculaCopia.Director = this.Pelicula.Director;
      this.PeliculaCopia.Pais = this.Pelicula.Pais;
      this.editing = true;
    },
    discard: function () {
      this.Pelicula.Titulo = this.PeliculaCopia.Titulo;
      this.Pelicula.Anyo = this.PeliculaCopia.Anyo;
      this.Pelicula.Director = this.PeliculaCopia.Director;
      this.Pelicula.Pais = this.PeliculaCopia.Pais;
      this.editing = false;
    },
    cleanForm: function() {
      this.Pelicula.Titulo = '';
      this.Pelicula.Anyo = '';
      this.Pelicula.Director = '';
      this.Pelicula.Pais = '';
      this.Pelicula.Id = '';
    },
    create: function () {
      var _this = this;
      $.ajax(
        {
          url : httpURL,
          type: "POST",
          data: {
            Titulo: this.Pelicula.Titulo,
            Anyo: this.Pelicula.Anyo,
            Director: this.Pelicula.Director,
            Pais: this.Pelicula.Pais,
          }

        })
        .done(function(data) {
          EventBus.$emit('updateListPelicula');
          let mensaje ='Película añadida con éxito.';
          EventBus.$emit('showMessage', mensaje);
          _this.addingNew = false;
        })
        .fail(function(data) {
          let mensaje = 'No se pudo crear la Película. Revise su conexión a Internet.';
          EventBus.$emit('showMessage', mensaje);
        });
      },
      update: function () {
        var _this = this;
        $.ajax(
          {
            url : httpURL + this.Pelicula.Id,
            type: "PUT",
            data: {
              Id: this.Pelicula.Id,
              Titulo: this.Pelicula.Titulo,
              Anyo: this.Pelicula.Anyo,
              Director: this.Pelicula.Director,
              Pais: this.Pelicula.Pais
            }
          })
          .done(function(data) {
            EventBus.$emit('updateListPelicula');
            let mensaje ='Película actualizada con éxito.';
            EventBus.$emit('showMessage', mensaje);
            _this.cleanForm();
            _this.editing = false;
          })
          .fail(function(data) {
            let mensaje = 'No se pudo actualizar la Película. Revise su conexión a Internet.';
            EventBus.$emit('showMessage', mensaje);
          });
        },
        remove: function () {
          var _this = this;
          $.ajax(
            {
              url : httpURL + this.Pelicula.Id,
              type: "DELETE",
              data: {Id: this.Pelicula.Id}
            })
            .done(function(data) {
              EventBus.$emit('updateListPelicula');
              _this.cleanForm();
              let mensaje ='Película eliminada con éxito.';
              EventBus.$emit('showMessage', mensaje);
              _this.editing = false;
            })
            .fail(function(data) {
              let mensaje = 'No se pudo eliminar la Película. Revise su conexión a Internet.';
              EventBus.$emit('showMessage', mensaje);
            });
          },
          load: function(id){
            var _this = this;
            $.ajax(
              {
                url : httpURL + id,
                type: "GET",
              })
              .done(function(data) {
                _this.Pelicula = data;
              })
              .fail(function(data) {
                let mensaje = 'No se pudo cargar la Película. Revise su conexión a Internet.';
                EventBus.$emit('showMessage', mensaje);
              });
            },
          },

          data: function () {
            return {
              editing: false,
              addingNew: false,
              Pelicula: {
                Id: '',
                Titulo: '',
                Anyo: '',
                Director: '',
                Pais: ''
              },
              PeliculaCopia: {
                Id: '',
                Titulo: '',
                Anyo: '',
                Director: '',
                Pais: ''
              }
            }
          },

          mounted: function() {
            EventBus.$on('peliculaSelected', function(id) {
              this.load(id);
              this.editing = false;
              this.addingNew = false;
            }.bind(this));
          }
        }

        </script>
