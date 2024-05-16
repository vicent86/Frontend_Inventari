<template>
  <h1>Editar Producte</h1>
  <div class="container mt-3">
    <h2>Editar Element</h2>
    <form @submit.prevent="editarProducto">
      <div class="form-group">
        <label for="nombre">Nombre:</label>
        <input
          v-model="nuevoProducto.data.nombre"
          type="text"
          class="form-control"
          id="nombre"
          placeholder="Introdueix el nom del producte"
        />
      </div>

      <div class="form-group">
        <label for="imagen" class="form-label">Imagen:</label>
        <input
          @change="handleFileUpload"
          type="file"
          class="form-control"
          id="imagen"
        />
      </div>

      <div class="form-group">
        <label for="descripcion">Descripción:</label>
        <textarea
          v-model="nuevoProducto.data.descripcion"
          class="form-control"
          id="descripcion"
          placeholder="Introduix la descripcio"
        ></textarea>
      </div>

      <div class="form-group">
        <label for="precio">Precio:</label>
        <input
          v-model="nuevoProducto.data.precio"
          type="text"
          class="form-control"
          id="precio"
          placeholder="Introduix el preu del producte"
        />
      </div>

      <div class="form-group">
        <label for="categoria_id">Categoría ID:</label>
        <input
          v-model="nuevoProducto.data.categoria_id"
          type="text"
          class="form-control"
          id="categoria_id"
        />
      </div>

      <div class="form-group">
        <label for="estado">Estado:</label>
        <input
          type="checkbox"
          class="form-check-input"
          id="estado"
          v-model="nuevoProducto.data.estado"
        />
        <span
          class="badge"
          :class="{
            'bg-success': nuevoProducto.data.estado,
            'bg-danger': !nuevoProducto.data.estado,
          }"
        >
          {{ nuevoProducto.data.estado ? "Activo" : "Inactivo" }}
        </span>
      </div>

      <div class="form-group">
        <label for="id_proveedor">ID Proveedor:</label>
        <input
          v-model="nuevoProducto.data.id_proveedor"
          type="text"
          class="form-control"
          id="id_proveedor"
        />
      </div>

      <div class="form-group">
        <label for="iva">IVA:</label>
        <input
          v-model="nuevoProducto.data.iva"
          type="text"
          class="form-control"
          id="iva"
          placeholder="Introduix el IVA del producte"
        />
      </div>

      <button type="submit" class="btn btn-primary">
        Actualizar Producto
      </button>
      <div
        class="alert alert-success"
        role="alert"
        v-if="productoActualizadoCorrectamente"
      >
        Producto editado correctamente
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "EditarProducto",
  data() {
    return {
      nuevoProducto: {
        data: {
          id: "",
          nombre: "",
          imagen: "",
          descripcion: "",
          precio: "",
          categoria_id: "",
          estado: false,
          id_proveedor: "",
          iva: "",
        },
      },
      productoActualizadoCorrectamente: false,
      selectedFile: null,
    };
  },
  props: {
    base_url: String,
  },
  methods: {
    handleFileUpload(event) {
      this.selectedFile = event.target.files[0];
    },
    async editarProducto() {
      let formData = new FormData();
      formData.append("nombre", this.nuevoProducto.data.nombre);
      if (this.selectedFile) {
        formData.append("imagen", this.selectedFile);
      }
      formData.append("descripcion", this.nuevoProducto.data.descripcion);
      formData.append("precio", this.nuevoProducto.data.precio);
      formData.append("categoria_id", this.nuevoProducto.data.categoria_id);
      formData.append("estado", this.nuevoProducto.data.estado ? 1 : 0);
      formData.append("id_proveedor", this.nuevoProducto.data.id_proveedor);
      formData.append("iva", this.nuevoProducto.data.iva);

      try {
        const response = await fetch(
          `${this.base_url}/productos/${this.nuevoProducto.id}`,
          {
            method: "PUT",
            body: formData,
          }
        );

        if (!response.ok) {
          throw new Error(`HTTP error! Status: ${response.status}`);
        }

        this.productoActualizadoCorrectamente = true;
      } catch (error) {
        console.error("Error al actualizar el producto:", error.message);
      }
    },
    async getProducto(id) {
      try {
        const url = `${this.base_url}/productos/${id}`;
        console.log("Fetching product from:", url); // Verifica la URL
        const response = await fetch(url);
        if (!response.ok) {
          throw new Error(`HTTP error! Status: ${response.status}`);
        }
        const data = await response.json();
        console.log("Product data:", data); // Verifica los datos obtenidos
        this.nuevoProducto.data = data;
      } catch (error) {
        console.error("Error al obtener el producto:", error);
      }
    },
    toggleEstado() {
      this.nuevoProducto.data.estado = !this.nuevoProducto.data.estado;
    },
  },
  mounted() {
    const productId = this.$route.params.id;
    console.log("Producto ID:", productId); // Asegúrate de que se muestra un ID válido
    if (productId) {
      this.nuevoProducto.data.id = productId; // Asigna el ID del producto
      this.getProducto(productId);
    } else {
      console.error("No se ha proporcionado un ID de producto válido.");
    }
  },
};
</script>

<style scoped></style>
