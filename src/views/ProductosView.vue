<template>
    <div class="container">
      <table class="table table-striped">
        <thead>
          <tr>
            <th>Nombre</th>
            <th>Imagen</th>
            <th>Descripcion</th>
            <th>Precio</th>
            <th>Categoria_ID</th>
            <th>Estado</th>
            <th>ID_Proveedor</th>
            <th>IVA</th>
          </tr>
        </thead>
        <tbody>
          <tr v-if="producto">
            <td>{{ producto.nombre }}</td>
            <td> <img :src="producto.imagen" alt="imagen" width="100"></td>
            <td>{{ producto.descripcion }}</td>
            <td>{{ producto.precio }}</td>
            <td>{{ producto.categoria_id }}</td>
            <td>{{ producto.estado ? 'Activo' : 'Inactivo' }}</td>
            <td>{{ producto.id_proveedor }}</td>
            <td>{{ producto.iva }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
  export default {
    name: "ProductosView",
    data() {
      return {
        msg: "Producto",
        producto: null
      };
    },
    props: {
      base_url: String,
    },
    mounted() {
      // L'esdeveniment mounted es dispara quan el component s'ha muntat al DOM
      // Fem una crida a una API o realitzem altres operacions aquí
      this.id = this.$route.params.id;
      //console.log(this.id);
      this.getProductos(this.id);
    },
    methods: {
      async getProductos(id) {
        try {
          let url = `${this.base_url}/productos/${id}`;
          const response = await fetch(url);
          if (!response.ok) {
            throw new Error(`Error al obtener el producto (código: ${response.status})`);
          }
          const data = await response.json();
          this.producto = data.data || data;
        } catch (error) {
          console.error(error);
          // Maneja el error mostrando un mensaje al usuario o redirigiéndolo a otra página
        }
      },
    },
  };
  </script>
  
<style scoped>
  img {
    max-width: 100%;
    height: auto;
  }
</style>
  