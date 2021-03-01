<template>

  <section class="contenido">
    <b-container fluid>
      <b-row class="p-3">
        <b-col lg="8">
            <b-form-input
            v-model="tituloNota"
            @keyup.enter="anadirNota"
            placeholder="Introduce texto nota..."
            >
            </b-form-input>
        </b-col>
        <b-col 
        class="mt-2 mt-lg-0"
        lg="4">
            <b-form-input
            v-model = "empiezaPor"
            placeholder="Introduce texto nota a buscar..."
            >
            </b-form-input>
        </b-col>
      </b-row>
      <b-row class="tareas ml-3">
        <div class="divCompletadas">
          <p> {{totalPendientes}} tareas pendiendes de un total de {{totalTareas}} |</p>
          <p @click="borrarTareasCompletadas" class="tareasCompletas ml-1">Borrar tareas completadas</p>
        </div>
        <div class="ordenar">
          <b-form-select @change="cambiarOrden" v-model="selected" class="mb-3">
              <b-form-select-option :value="null">Selecciona opción para ordenar</b-form-select-option>
              <b-form-select-option value="a">Alfabeticamente asc</b-form-select-option>
              <b-form-select-option value="b" >Alfabeticamente desc</b-form-select-option>
              <b-form-select-option value="c">Prioridad alta</b-form-select-option>
              <b-form-select-option value="d" >Prioridad baja</b-form-select-option>
              <b-form-select-option value="e">Fecha ascendente</b-form-select-option>
              <b-form-select-option value="f" >Fecha descendente</b-form-select-option>
          </b-form-select>
  </div>


      </b-row>
      <b-row>
        <b-col 
        offset-lg="2"
        lg="8">
        
          <nota 
      v-for = "nota in listaRecordatoriosFiltrada"
      :key = "nota.fecha.toString()"
      :nota="nota"
      />
        </b-col>
      </b-row> 
    </b-container>
  </section>

</template>

<script lang="js">
  import nota from "./nota.vue";
  import { db } from '../db.js'

  export default  {
    name: 'contenido',
    components: {
      nota
    },
    props: [],
    mounted () {
    },
    data () {
      return {
        listaNotas : [],
        tituloNota:"",
        empiezaPor:"",
        selected: null
      }
    },
    methods: {
      anadirNota:function(){
        db.collection('notas').add({
          titulo: this.tituloNota,
          prioridad:0,
          fecha: new Date(),
          completado:false
        })
        this.tituloNota="";
      },
      borrarTareasCompletadas: function()
    {
      this.listaNotas.forEach(nota => {
        if(nota.completado)
          db.collection('notas').doc(nota.id).delete()
      });
    },
      cambiarOrden:function() {
        if(this.selected == "a")
          this.$bind('listaNotas',db.collection('notas').orderBy('titulo','asc'));
        else if(this.selected == "b")
          this.$bind('listaNotas',db.collection('notas').orderBy('titulo','desc'));
        else if(this.selected == "c")
          this.$bind('listaNotas',db.collection('notas').orderBy('prioridad','desc'));
        else if(this.selected == "d")
          this.$bind('listaNotas',db.collection('notas').orderBy('prioridad','asc'));
        else if(this.selected == "e")
          this.$bind('listaNotas',db.collection('notas').orderBy('fecha','asc'));
        else if(this.selected == "f")
          this.$bind('listaNotas',db.collection('notas').orderBy('fecha','desc'));
      }
    },
    computed: {
      listaRecordatoriosFiltrada: function()
    {
      let listaFiltrada;
      if(this.empiezaPor == "")
        listaFiltrada =  this.listaNotas;
      else
        listaFiltrada = this.listaNotas.filter((recordatorio)=>
        {
          if(recordatorio.titulo.indexOf(this.empiezaPor) >= 0 )
            return true;
          else
            return false;
        });
      return listaFiltrada;
    },
    totalPendientes: function()
    {
      let total = 0;
        
      for(let i=0;i<this.listaNotas.length;i++)
        if(!this.listaNotas[i].completado)
          total++;

      return total;
    },
      totalTareas: function()
    {
      return this.listaNotas.length;
    }
    },
    firestore:
  { 
    listaNotas: db.collection('notas')
  }
}


</script>

<style scoped>
  .contenido {
    min-height: 80%;
    background-color: black;
  }

  .tareas{
    display: flex;
    justify-content: space-around;
  }
  .divCompletadas{
    display: flex;
    flex-direction: row;
  }

  p{
    color: #f1f1f1;
  }

  .tareasCompletas{
    color: orange;
    cursor: pointer;
  }
  input{
    background-color: #f1f1f1;
  }
</style>
