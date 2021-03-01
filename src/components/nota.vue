<template lang="html">

  <section class="nota">
    <b-container fluid class="d-flex align-items-center justify-content-between p-4 my-2">
      <div>
        <div>
          <p @click="tareaCompletada" v-if="!isDisabled" class="text-left texto" :class="{tareaCompletada: nota.completado}">{{nota.titulo}}</p>
          <input class="my-2" @keyup.enter="cambiarNombreNota" v-model ="editarNota" v-if ="isDisabled" type="text">
        </div>
        <div class="d-flex align-items-center">
          <p>Prioridad: </p>
          <p id="0" class="ml-1 p-1 boton" @click="darPrioridad" :class="{prioridad1:nota.prioridad==0}">Baja</p>
          <p id="1" class="ml-1 p-1 boton" @click="darPrioridad" :class="{prioridad2:nota.prioridad==1}">Media</p>
          <p id="2" class="ml-1 p-1 boton" @click="darPrioridad" :class="{prioridad3:nota.prioridad==2}">Alta</p>
        </div>
      </div>
      <div>
        <b-button @click="cambiarEstadoEditar" size="sm" class="ml-1" :disabled="isDisabled"><font-awesome-icon icon="edit"/></b-button>
        <b-button @click="borrarNota" size="sm" class="ml-2"><font-awesome-icon icon="trash-alt"/></b-button>
      </div>
    </b-container>
  </section>

</template>

<script lang="js">
import { db } from '../db.js'


  export default  {
    name: 'nota',
    props: [
      "nota"
    ],
    mounted () {

    },
    data () {
      return {
        isDisabled :false,
        editarNota: this.nota.titulo
      }
    },
    methods: {
      borrarNota(){
        db.collection('notas')
          .doc(this.nota.id)
          .delete()
      },
      cambiarEstadoEditar(){
        this.isDisabled = !this.isDisabled;
      },
      cambiarNombreNota(){
        db.collection('notas')
          .doc(this.nota.id)
          .update({titulo:this.editarNota})
        this.cambiarEstadoEditar();
      },
      tareaCompletada:function () {
        this.nota.completado = !this.nota.completado;
        db.collection('notas')
          .doc(this.nota.id)
          .update({completado:true})
      },
      darPrioridad:function (e) {
        let prioridad = e.target.id;
        db.collection('notas')
          .doc(this.nota.id)
          .update({prioridad:prioridad})
      },
    },
    computed: {

    },
      firestore:
  { 
    archivos: db.collection('documentos')
  }
}


</script>

<style scoped >
  .nota {
    background-color:#3e3d3d;
    color: #f1f1f1;
    border-radius: 40px;
  }

  .texto{
    font-size: 2rem;
  }

  input{
    background-color: #f1f1f1;;
    font-size: 2rem;
  }

  .boton{
    background-color: rgb(157 157 157);
    color: #f1f1f1;
    border-radius: 5px;
    width: 4rem;
    cursor: pointer;
    font-size: .75rem;
  }

  svg{
    font-size: 1.5rem;
  }
  .prioridad1{
    background-color: #a5a51d;
  }

  .prioridad2{
    background-color: #065b06;
  }

  .prioridad3{
    background-color:#8c0404;
  }

  .tareaCompletada{
    color:green;
    text-decoration: line-through;
  }
</style>
