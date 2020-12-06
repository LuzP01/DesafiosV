<template>
    <div>
     <h1>Mantenedor de productos</h1>
    <form id="app" @submit="checkForm" action="https://vuejs.org/" method="post" class="container mx-auto mt-4">
      <p v-if="errors.length">
    <b>Por favor, corrija el(los) siguiente(s) error(es):</b>
    <ul>
      <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
    </ul>
  </p>
  <div class="form-group">
    <label for="exampleInputName">Nombre</label>
    <input v-model="nombre" @keyup.enter="crearProducto" type="text" class="form-control" id="nombre">
  </div>
  <div class="form-group">
    <label for="exampleInputDescription">Descripcion</label>
    <input v-model="descripcion" @keyup.enter="crearProducto" type="text" class="form-control" id="descripcion">
  </div>
  <div class="form-group">
    <label for="exampleInputPrecio">Precio</label>
    <input v-model="precio" @keyup.enter="crearProducto" type="text" class="form-control" id="precio" min="1">
  </div>
  <input @click="crearProducto" type="button" value="Añadir" class="btn btn-success">
</form>
<br>
<h3> Lista de productos</h3>
<section class="data">
<table class="table table-bordered">
  <thead>
    <tr>
      <th scope="col">#</th>
      <th scope="col">Nombre</th>
      <th scope="col">Descripción</th>
      <th scope="col">Precio</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(producto, index) in productos" v-bind:key="producto">
      <td>{{producto.id}}</td>
      <td> 
        <span v-if="formActualizar && idActualizar == index">
          <input v-model="nombreActualizar" type="text" class="form-control">
        </span>
        <span v-else>
          {{producto.nombre}}
        </span>
      </td>
      <td>
        <span v-if="formActualizar && idActualizar == index">
          <input v-model="descripcionActualizar" type="text" class="form-control">
        </span>
        <span v-else>
          {{producto.descripcion}}
        </span>
      </td>
      <td>
        <span v-if="formActualizar && idActualizar == index">
          <input v-model="precioActualizar" type="text" class="form-control">
        </span>
        <span v-else>
          {{producto.precio}}
        </span>
      </td>
      <span v-if="formActualizar && idActualizar == index">
        <button @click="guardarActualizacion(index)" class="btn btn-success">Guardar</button>
      </span>
      <span v-else>
        <button @click="verFormActualizar(index)" class="btn btn-warning">Editar</button>
        <button @click="eliminarProducto(index)" class="btn btn-danger">Eliminar</button>
      </span>
    </tr>
  </tbody>
</table>
</section>
    </div>
</template>

<script>


  export default {
  name: "App",
  components: {},

  data: ()=>({

    nombre:null,
    descripcion:null,
    precio:0,
    formActualizar:false,
    idActualizar: -1,
    nombreActualizar:'',
    descripcionActualizar:'',
    precioActualizar:'',
    errors: [],
    productos: []
    
  

  }),
  methods:{

    crearProducto: function(){
      this.productos.push({
        id: this.id,
        nombre: this.nombre,
        descripcion: this.descripcion,
        precio: this.precio
      });
      this.nombre= '';
      this.descripcion='';
      this.precio='';

    },
    verFormActualizar: function(producto_id){
      this.idActualizar= producto_id;
      this.nombreActualizar=this.productos[producto_id].nombre;
      this.descripcionActualizar=this.productos[producto_id].descripcion;
      this.precioActualizar=this.productos[producto_id].precio;
      this.formActualizar=true;
    },
    eliminarProducto: function(producto_id){
      this.productos.splice(producto_id, 1);

    },
    guardarActualizacion: function(producto_id){
      this.formActualizar=false;
      this.productos[producto_id].nombre = this.nombreActualizar;
      this.productos[producto_id].descripcion = this.descripcionActualizar;
      this.productos[producto_id].precio = this.precioActualizar;

    },
    checkForm: function (e){
      if(this.nombre && this.descripcion){
        return true;
      }
      this.errors = [];

      if(!this.nombre){
        this.errors.push('El nombre es campo requerido');
      }
      if(this.descripcion){
        this.errors.push('La descripción es campo requerido');
      }
      e.preventDefault();
    }
  },
};
</script>

<style>
#app {
  
}
</style>
