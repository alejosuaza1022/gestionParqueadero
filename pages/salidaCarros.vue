 
<template>
  <div class="content">
    <b-container>
      <!-- Content here -->

      <h1>Formulario de vehiculos </h1>

      <b-table striped hover :items="lista_vehiculos">
        <template v-slot:cell(acciones)="row">
          <b-button size="sm" @click="dar_salida(row)" class="mr-2 acciones">dar salida</b-button>
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
import moment from 'moment'
export default {
  data() {
    return {
    hoy:null,
    lista_vehiculos:null
  };
  },
  
  mounted() {
    this.created();
  

  },
  methods: {
    created() {
      let datosLS = JSON.parse(localStorage.getItem("registroCarro"));
      if (!datosLS) this.lista_vehiculos = [];
      else this.lista_vehiculos = datosLS;
    },
    dar_salida({item}){
      let array_fecha  = item.fecha_ingreso.split("-",3)
      let to_parse_fecha1 = new Date(array_fecha) 
      let horas = item.hora_ingreso.split(":",3)
      to_parse_fecha1 .setHours(horas[0],horas[1],horas[2])
      let fecha1 = moment(to_parse_fecha1,"YYYY-MM-DD HH:mm:ss")
      let to_parse_hoy = new Date()
      let fecha2 = moment(to_parse_hoy,"YYYY-MM-DD HH:mm:ss")
      let diff = fecha2.diff(fecha1,'h')
      let bool = confirm(" el tiempo que estuvo fue : " + diff + " y debe pagar : " + diff*item.valor)
      if(bool)
        this.eliminar_vehiculo({item})
        
    },
     eliminar_vehiculo({item}){
      let posicion = this.lista_vehiculos.findIndex(
        vehiculo => vehiculo.placa == item.placa
      );
      this.lista_vehiculos.splice(posicion, 1);
      this.agregarInfoLS()
    },


    agregarInfoLS() {
      localStorage.setItem(
        "registroCarro",
        JSON.stringify(this.lista_vehiculos)
      );
    },
  }
};
</script>