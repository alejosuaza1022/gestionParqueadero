 
<template>
  <div class="content">
    <b-container>
      <!-- Content here -->

      <h1>Formulario de vehiculos {{mensaje}}</h1>

      <b-form action="javascript:void(0)" @submit="crearVehiculo">
        <b-form-group label="seleccione el valor de vehiculo">
          <b-form-select v-model="vehiculo.valor" :options="opciones_valor" v-bind:required="true"></b-form-select>
        </b-form-group>
        <b-form-group label="placa" label-for="placa">
          <b-form-input
            class="form-control"
            v-bind:required="true"
            v-model="vehiculo.placa"
            type="text"
            placeholder="ingrese la placa del vehiculo"
            id="placax"
          />
        </b-form-group>

        <b-form-group label="fecha de realización" label-for="fecha">
          <b-form-input
            class="form-control"
            v-model="vehiculo.fecha_ingreso"
            type="date"
            data-date-format="DD MMMM YYYY"
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
          <b-form-timepicker v-model="vehiculo.hora_ingreso" v-bind:required="true" locale="en"></b-form-timepicker>
        </b-form-group>

        <b-button
          type="submit"
          variant="primary"
          v-if="!enEdicion"
          class="submit-button"
        >Crear vehiculo</b-button>
        <b-button
          @click="actualizar_vehiculo()"
          variant="primary"
          v-else
          class="submit-button"
        >Actualizar vehiculo</b-button>
      </b-form>

      <b-table striped hover :items="lista_vehiculos">
        <template v-slot:cell(acciones)="row">
          <b-button size="sm" @click="cargar_vehiculo(row)" class="mr-2 acciones">Modificar</b-button>
          <b-button size="sm" @click="eliminar_vehiculo(row)" class="mr-2 acciones">Eliminar</b-button>
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
      enEdicion: false,
      lista_vehiculos: [],
      mensaje: "sadasdas",
      bool: false,
      vehiculo: {
        placa: null,
        fecha_ingreso: null,
        color: null,
        marca: null,
        ciudad_placa: null,
        valor: null,
        hora_ingreso: null,
        acciones: true
      },
      opciones_valor: [
        { value: null, text: "seleccione una opción" },
        { value: 8000, text: "CARRO" },
        { value: 4000, text: "MOTO" },
        { value: 2000, text: "BICICLETA" },
        { value: 21000, text: "CAMION" }
      ]
    };
  },
  mounted() {
    this.created();
      console.log(this.opciones_valor[0])

  },
  methods: {
  
    crearVehiculo() {
      let bool = this.vehiculo.hora_ingreso !== null;
      if (bool) {
        if (
          this.lista_vehiculos.findIndex(
            vehiculo =>
              vehiculo.placa.toUpperCase() == this.vehiculo.placa.toUpperCase()
          ) === -1
        ) {
          this.lista_vehiculos.push(this.vehiculo);
          this.agregarInfoLS(), this.limpiar();
        } else alert("repetida");
      }
      else alert("llene todos los campos");
    },
    limpiar() {
      this.vehiculo = {
        placa: null,
        fecha_ingreso: null,
        color: null,
        marca: null,
        ciudad_placa: null,
        valor: null,
        hora_ingreso: null,
        acciones: true
      };
    },
    created() {
      let datosLS = JSON.parse(localStorage.getItem("registroCarro"));
      if (!datosLS) this.lista_vehiculos = [];
      else this.lista_vehiculos = datosLS;
    },
    agregarInfoLS() {
      localStorage.setItem(
        "registroCarro",
        JSON.stringify(this.lista_vehiculos)
      );
    },
    cargar_vehiculo({ item }) {
      let auxEvalua = this.lista_vehiculos.find(
        evaluador => evaluador.placa == item.placa
      );
      this.enEdicion = true;

      this.vehiculo = Object.assign({}, auxEvalua);
      console.log(this.vehiculo.hora_ingreso);
    },
    actualizar_vehiculo() {
      this.enEdicion = false;
      let posicion = this.lista_vehiculos.findIndex(
        vehiculo => vehiculo.placa == this.vehiculo.placa
      );
      this.lista_vehiculos.splice(posicion, 1, this.vehiculo);
      this.limpiar();
      this.agregarInfoLS();
    },
    eliminar_vehiculo() {
      let posicion = this.lista_vehiculos.findIndex(
        vehiculo => vehiculo.placa == this.vehiculo.placa
      );
      this.lista_vehiculos.splice(posicion, 1);
      this.agregarInfoLS();
    }
  }
};
</script>