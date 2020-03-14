 
<template>
  <div class="content">
    <b-container>
      <!-- Content here -->

      <h1>Formulario de vehiculos {{mensaje}}</h1>

      <b-form action="javascript:void(0)" @submit="crearVehiculo">
        <b-form-group label="seleccione el tipo de vehiculo">
          <b-form-select v-model="vehiculo.tipo" :options="opciones_tipo"></b-form-select>
        </b-form-group>
        <b-form-group label="placa" label-for="placa">
          <b-form-input
            class="form-control"
            v-bind:required="true"
            v-model="vehiculo.placa"
            type="number"
            placeholder="ingrese la placa del vehiculo"
            id="placax"
          />
        </b-form-group>

        <b-form-group label="fecha de realización" label-for="fecha">
          <b-form-input
            class="form-control"
            v-model="vehiculo.fecha_ingreso"
            type="date"
            v-bind:required="true"
            placeholder="Ingrese la fecha de realización"
            id="fecha"
          />
        </b-form-group>
        <b-form-group label="color carro" label-for="color">
          <b-form-input
            class="form-control"
            v-model="vehiculo.color"
            v-bind:required="true"
            placeholder="ingrese el color del carro"
            id="color"
          />
        </b-form-group>
        <b-form-group label="marca carro" label-for="marca">
          <b-form-input
            class="form-control"
            v-model="vehiculo.marca"
            v-bind:required="true"
            placeholder="ingrese la marca del carro"
            id="marca"
          />
        </b-form-group>
        <b-form-group label="ciudad de la placa" label-for="ciudad_placa">
          <b-form-input
            class="form-control"
            v-model="vehiculo.ciudad_placa"
            v-bind:required="true"
            placeholder="ingrese la ciudad de la placa"
            id="ciudad_placa"
          />
        </b-form-group>
        <b-form-group>
          <b-form-timepicker v-model="vehiculo.horaingreso" locale="en"></b-form-timepicker>
        </b-form-group>

        <b-button
          type="submit"
          variant="primary"
          v-if="!enEdicion"
          class="submit-button"
        >Crear vehiculo</b-button>
        <b-button
          @click="actualizarEstudiante()"
          variant="primary"
          v-else
          class="submit-button"
        >Actualizar vehiculo</b-button>
      </b-form>

      <b-table striped hover :items="lista_vehiculoes">
        <template v-slot:cell(acciones)="row">
          <b-button size="sm" @click="cargarEstudiante(row)" class="mr-2 acciones">Modificar</b-button>
          <b-button size="sm" @click="eliminarEstudiante(row)" class="mr-2 acciones">Eliminar</b-button>
        </template>
      </b-table>
    </b-container>
  </div>
</template>

<style>
@import "../assets/sizes.css";

textarea {
  min-height: 100px;
  text-decoration: none;
}

input:focus,
textarea:focus {
  border: 2px solid rgb(238, 36, 36) !important;
  box-shadow: 0 0 0px !important;
}
</style>


<script >
export default {
  data() {
    return {
      lista_vehiculos: [],
      mensaje: "sadasdas",
      vehiculo: {
        placa: null,
        fecha_ingreso: null,
        color: null,
        marca: null,
        ciudad_placa: null,
        tipo: null
      },
      opciones_tipo: [
        { value: null, text: "seleccione una opción" },
        { value: 8000, text: "CARRO" },
        { value: 4000, text: "MOTO" },
        { value: 2000, text: "BICICLETA" },
        { value: 21000, text: "CAMION" }
      ]
    };
  },
  methods: {
    crearVehiculo() {
      this.lista_vehiculos.push(this.vehiculo);
        this.limpiar()
},
    limpiar() {
      this.vehiculo = {
        placa: null,
        fecha_ingreso: null,
        color: null,
        marca: null,
        ciudad_placa: null,
        tipo: null
      };
    }
  }
};
</script>