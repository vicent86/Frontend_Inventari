<template>
  <div>
    <h1>{{ msg }}</h1>
  </div>
  <div class="container">
    <button
      type="button"
      class="btn btn-outline-info"
      @click="this.nuevoProveedor(proveedorIndex)"
    >
      Añadir Proveedor
    </button>
    <br />
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
        <template v-for="proveedor in proveedores.data" :key="proveedor.id">
          <tr>
            <td>{{ proveedor.nombre }}</td>
            <td>{{ proveedor.direccion }}</td>
            <td>{{ proveedor.email }}</td>
            <td>{{ proveedor.cif }}</td>
            <td>
              <span
                class="badge bg-success"
                v-if="proveedor.estado === 'Activo'"
                >Activo</span
              >
              <span class="badge bg-danger" v-else>Inactivo</span>
            </td>
            <td>{{ proveedor.cualificacion }} ★</td>
            <td>
              <button
                class="btn btn-outline-info"
                @click="VistaProveedor(proveedor.id)"
              >
                Vista
              </button>
              <button
                class="btn btn-outline-warning"
                @click="editarProveedor(proveedor.id)"
              >
                Editar
              </button>
              <button
                class="btn btn-outline-danger"
                @click="borrarProveedor(proveedor.id)"
              >
                Borrar
              </button>
            </td>
          </tr>
        </template>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "EditarProveedor",
  data() {
    return {
      msg: "Lista de Proveedores",
      id: 1,
      categoria: {
        id: 1,
        nombre: "Hermanos Gutierrez",
        direccion: "Poligono el Alternador,25,Llombai",
        email: "gutiehmnos@gmail.com",
        cif: "B123589W",
        estado: "Activo",
        cualificacion: 4,
      },
      proveedores: { data: [] },
    };
  },
  props: {
    base_url: String,
  },
  methods: {
    async getProveedores() {
      try {
        let url = this.base_url + "/proveedores";
        console.log(url);
        const response = await fetch(url);
        this.proveedores = await response.json();
        console.log("Els proveedors son:  ", this.proveedores);
      } catch (error) {
        console.error(error);
      }
    },
    async borrarProveedor(id) {
      try {
        const confirmacion = confirm(
          "¿Estás seguro de que deseas borrar este Proveedor?"
        );
        if (confirmacion) {
          await fetch(`${this.base_url}/proveedores/${id}`, {
            method: "DELETE",
          });

          await this.getProveedores();
          alert("Proveedor Borrado Correctamente");
        }
      } catch (error) {
        console.error(error);
      }
    },
    async nuevoProveedor(proveedorIndex) {
      this.$router.push({
        name: "nuevoproveedor",
        params: { id: proveedorIndex },
      });
    },
    async VistaProveedor(id) {
      this.$router.push({ name: "proveedor", params: { id: id } });
    },
    async editarProveedor(id) {
      this.$router.push({ name: "editarproveedor", params: { id: id } });
    },

    async actualizarEstadoProveedor(id, estado) {
      const index = this.proveedores.data.findIndex((p) => p.id === id);
      if (index !== -1) {
        this.proveedores.data[index].estado = estado;
      }
    },

    async generarCualificacion(cualificacion) {
  return new Promise((resolve) => {
    setTimeout(() => {
      let cualificacionesHTML = "";
      for (let i = 0; i < 5; i++) {
        if (i < cualificacion) {
          cualificacionesHTML += '<i class="fas fa-star"></i>';
        } else {
          cualificacionesHTML += '<i class="far fa-star"></i>';
        }
      }
      resolve(cualificacionesHTML);
    }, 0); // Simula una operación asíncrona con un pequeño retraso
  });
}
  },
  mounted() {
    this.id = this.$route.params.id;
    //console.log("ID de la categoria a mostrar es " + this.id);
    this.getProveedores();
  },
};
</script>

<style scoped></style>
