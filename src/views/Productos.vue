<template>
  <div>
    <h1>{{ msg }}</h1>
    <div class="container">
      <button
        type="button"
        class="btn btn-outline-info"
        @click="nuevoProducto"
      >
        Añadir Producto
      </button>
      <br />
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
            <th>Acciones</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="producto in productos" :key="producto.id">
            <td>{{ producto.nombre }}</td>
            <td>
              <img :src="producto.imagen" alt="imagen" width="100">
            </td>
            <td>{{ producto.descripcion }}</td>
            <td>{{ producto.precio }}</td>
            <td>{{ producto.categoria_id }}</td>
            <td>
              <span class="badge bg-success" v-if="producto.estado === 'Activo'">
                Activo
              </span>
              <span class="badge bg-danger" v-else>
                Inactivo
              </span>
            </td>
            <td>{{ producto.id_proveedor }}</td>
            <td>{{ producto.iva }}</td>
            <td>
              <button
                class="btn btn-outline-info"
                @click="VistaProducto(producto.id)"
              >
                Vista
              </button>
              <button
                class="btn btn-outline-warning"
                @click="editarProducto(producto.id)"
              >
                Editar
              </button>
              <button
                class="btn btn-outline-danger"
                @click="borrarProducto(producto.id)"
              >
                Borrar
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "EditarProductos",
  data() {
    return {
      msg: "Lista de Productos",
      productos: [],
    };
  },
  props: {
    base_url: String,
  },
  methods: {
    async getProductos() {
      try {
        const url = `${this.base_url}/productos`;
        const response = await fetch(url);
        const data = await response.json();
        this.productos = data.data || data;
        console.log("Los productos son: ", this.productos);
      } catch (error) {
        console.error("Error al obtener productos:", error);
      }
    },
    async borrarProducto(id) {
      try {
        const confirmacion = confirm("¿Estás seguro de que deseas borrar este Producto?");
        if (confirmacion) {
          await fetch(`${this.base_url}/productos/${id}`, {
            method: "DELETE",
          });
          await this.getProductos();
          alert("Producto Borrado Correctamente");
        }
      } catch (error) {
        console.error("Error al borrar producto:", error);
      }
    },
    nuevoProducto() {
      this.$router.push({ name: "nuevoproducto" });
    },
    VistaProducto(id) {
      this.$router.push({ name: "producto", params: { id } });
    },
    editarProducto(id) {
      this.$router.push({ name: "editarproducto", params: { id } });
    },
  },
  mounted() {
    this.getProductos();
  },
};
</script>

<style scoped>
img {
  max-width: 100%;
  height: auto;
}
</style>
