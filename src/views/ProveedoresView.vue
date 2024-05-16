<template>
  <div class="container">
    <table class="table table-striped">
      <thead>
        <tr>
          <th>Nombre</th>
          <th>Direccion</th>
          <th>Email</th>
          <th>CIF</th>
          <th>Estado</th>
          <th>Cualificacion</th>
        </tr>
      </thead>
      <tbody>
        <tr v-if="proveedor.data">
          <td>{{ proveedor.data.nombre }}</td>
          <td>{{ proveedor.data.direccion }}</td>
          <td>{{ proveedor.data.email }}</td>
          <td>{{ proveedor.data.cif }}</td>
          <td>{{ proveedor.data.estado ? 'Activo' : 'Inactivo' }}</td>
          <td>{{ proveedor.data.cualificacion }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "ProveedoresView",
  data() {
    return {
      msg: "Proveedor",
      proveedor: {
        // data: {
        //   id: "",
        //   nombre: "",
        //   direccion: "",
        //   email: "",
        //   cif: "",
        //   estado: "",
        //   cualificacion: "",
        // },
        data: null
      },
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
    this.getProveedores(this.id);
  },
  methods: {
    async getProveedores(id) {
      try {
        let url = `${this.base_url}/proveedores/${id}`;
        const response = await fetch(url);
        if (!response.ok) {
          throw new Error(`Error al obtener el proveedor (código: ${response.status})`);
        }
        const data = await response.json();
        this.proveedor.data = data;
      } catch (error) {
        console.error(error);
        // Maneja el error mostrando un mensaje al usuario o redirigiéndolo a otra página
      }
    },
  },
};
</script>

<style scoped></style>
