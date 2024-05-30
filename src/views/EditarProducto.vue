<template>
  <h1>Editar Producto</h1>
  <div class="container mt-3">
    <h2>Modificar Producto</h2>
    <form @submit.prevent="editarProducto">
      <div class="form-group">
        <label for="nombre">Nombre:</label>
        <input
          v-model="nuevoProducto.nombre"
          type="text"
          class="form-control"
          id="nombre"
          placeholder="Introduce el nombre del producto"
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
          v-model="nuevoProducto.descripcion"
          class="form-control"
          id="descripcion"
          placeholder="Introduce la descripción"
        ></textarea>
      </div>

      <div class="form-group">
        <label for="precio">Precio:</label>
        <input
          v-model="nuevoProducto.precio"
          type="number"
          step="0.01"
          class="form-control"
          id="precio"
          placeholder="Introduce el precio del producto"
        />
      </div>

      <div class="form-group">
        <label for="categoria_id">Categoría:</label>
        <select
          v-model="nuevoProducto.categoria_id"
          class="form-control"
          id="categoria_id"
        >
          <option
            v-for="categoria in categorias"
            :key="categoria.id"
            :value="categoria.id"
          >
            {{ categoria.nombre }}
          </option>
        </select>
      </div>

      <div class="form-group">
        <label for="estado">Estado:</label>
        <input
          v-model="nuevoProducto.estado"
          type="checkbox"
          class="form-check-input"
          id="estado"
        />
        <span
          class="badge"
          :class="{
            'bg-success': nuevoProducto.estado,
            'bg-danger': !nuevoProducto.estado,
          }"
        >
          {{ nuevoProducto.estado ? "Activo" : "Inactivo" }}
        </span>
      </div>

      <div class="form-group">
        <label for="id_proveedor">Proveedor:</label>
        <select
          v-model="nuevoProducto.id_proveedor"
          class="form-control"
          id="id_proveedor"
        >
          <option
            v-for="proveedor in proveedores"
            :key="proveedor.id"
            :value="proveedor.id"
          >
            {{ proveedor.nombre }}
          </option>
        </select>
      </div>

      <div class="form-group">
        <label for="iva">IVA:</label>
        <input
          v-model="nuevoProducto.iva"
          type="number"
          step="0.01"
          class="form-control"
          id="iva"
          placeholder="Introduce el IVA del producto"
        />
      </div>

      <button type="submit" class="btn btn-primary">Guardar Cambios</button>
      <div
        class="alert alert-success"
        role="alert"
        v-if="productoActualizadoCorrectamente"
      >
        Producto actualizado correctamente
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
      productoActualizadoCorrectamente: false,
      selectedFile: null,
      categorias: [],
      proveedores: [],
    };
  },
  props: {
    base_url: String,
  },
  methods: {
    async fetchCategorias() {
      try {
        const response = await fetch(`${this.base_url}/categorias`);
        if (!response.ok) {
          throw new Error(`Error al cargar las categorías: ${response.status}`);
        }
        const data = await response.json();
        this.categorias = data.data;
      } catch (error) {
        console.error("Error al cargar las categorías:", error);
      }
    },
    async fetchProveedores() {
      try {
        const response = await fetch(`${this.base_url}/proveedores`);
        if (!response.ok) {
          throw new Error(
            `Error al cargar los proveedores: ${response.status}`
          );
        }
        const data = await response.json();
        this.proveedores = data.data || data;
      } catch (error) {
        console.error("Error al cargar los proveedores:", error);
      }
    },
    handleFileUpload(event) {
      this.selectedFile = event.target.files[0];
    },
    async editarProducto() {
      try {
        let formData = new FormData();
        formData.append("nombre", this.nuevoProducto.nombre);
        if (this.selectedFile) {
          formData.append("imagen", this.selectedFile);
        }
        formData.append("descripcion", this.nuevoProducto.descripcion);
        formData.append("precio", this.nuevoProducto.precio);
        formData.append("categoria_id", this.nuevoProducto.categoria_id);
        formData.append("estado", this.nuevoProducto.estado ? "1" : "0");
        formData.append("id_proveedor", this.nuevoProducto.id_proveedor);
        formData.append("iva", this.nuevoProducto.iva);

        console.log("Enviando datos del producto:", this.nuevoProducto);
        const response = await fetch(
          `${this.base_url}/productos/${this.nuevoProducto.id}`,
          {
            method: "PUT",
            body: new URLSearchParams(formData),
          }
        );

        if (!response.ok) {
          const errorText = await response.text();
          throw new Error(
            `HTTP error! Status: ${response.status}, ${errorText}`
          );
        }

        const responseData = await response.json();
        console.log("Respuesta del servidor:", responseData);

        this.productoActualizadoCorrectamente = true;

        this.nuevoProducto = {
          id: "",
          nombre: "",
          imagen: "",
          descripcion: "",
          precio: "",
          categoria_id: "",
          estado: false,
          id_proveedor: "",
          iva: "",
        };

        // Actualizar la lista de productos en el frontend
        this.$emit("producto-actualizado", responseData);
      } catch (error) {
        console.error("Error al actualizar el producto:", error.message);
      }
    },

    async getProducto(id) {
      try {
        const url = `${this.base_url}/productos/${id}`;
        const response = await fetch(url);
        if (!response.ok) {
          throw new Error(`HTTP error! Status: ${response.status}`);
        }
        const data = await response.json();
        this.nuevoProducto = data.data;
        this.nuevoProducto.estado = !!data.data.estado;
      } catch (error) {
        console.error("Error al obtener el producto:", error);
      }
    },
  },
  async mounted() {
    const productId = this.$route.params.id;
    if (productId) {
      this.nuevoProducto.id = productId;
      await this.getProducto(productId);
    } else {
      console.error("No se ha proporcionado un ID de producto válido.");
    }
    await this.fetchCategorias();
    await this.fetchProveedores();
  },
};
</script>

<style scoped></style>
