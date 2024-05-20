<template>
  <h1>Crear Producto</h1>
  <div class="container mt-3">
    <h2>Nuevo Producto</h2>
    <form @submit.prevent="crearProducto">
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
          type="text"
          class="form-control"
          id="precio"
          placeholder="Introduce el precio del producto"
        />
      </div>

      <div class="form-group">
        <label for="categoria_id">Categoría:</label>
        <select v-model="nuevoProducto.categoria_id" class="form-control" id="categoria_id">
          <option v-for="categoria in categorias" :key="categoria.id" :value="categoria.id">
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
      </div>
      <div class="form-group">
        <label for="id_proveedor">Proveedor:</label>
        <select v-model="nuevoProducto.id_proveedor" class="form-control" id="id_proveedor">
          <option v-for="proveedor in proveedores" :key="proveedor.id" :value="proveedor.id">
            {{ proveedor.nombre }}
          </option>
        </select>
      </div>
      
      <div class="form-group">
        <label for="iva">IVA:</label>
        <input
          v-model="nuevoProducto.iva"
          type="text"
          class="form-control"
          id="iva"
          placeholder="Introduce el IVA del producto"
        />
      </div>

      <button type="submit" class="btn btn-primary">Añadir Producto</button>
      <div
        class="alert alert-success"
        role="alert"
        v-if="productoCreadoCorrectamente"
      >
        Producto creado correctamente
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "CrearProducto",
  data() {
    return {
      nuevoProducto: {
        nombre: "",
        descripcion: "",
        precio: "",
        categoria_id: "",
        estado: false,
        id_proveedor: "",
        iva: "",
      },
      productoCreadoCorrectamente: false,
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
        console.log("Categorías cargadas:", data);
        this.categorias = data.data; 
      } catch (error) {
        console.error("Error al cargar las categorías:", error);
      }
    },
    async fetchProveedores() {
      try {
        const response = await fetch(`${this.base_url}/proveedores`);
        if (!response.ok) {
          throw new Error(`Error al cargar los proveedores: ${response.status}`);
        }
        const data = await response.json();
        console.log("Proveedores cargados:", data);
        this.proveedores = data.data || data; 
      } catch (error) {
        console.error("Error al cargar los proveedores:", error);
      }
    },
    handleFileUpload(event) {
      this.selectedFile = event.target.files[0];
    },

    async crearProducto() {
      if (!this.selectedFile) {
        console.error("No se ha seleccionado ninguna imagen.");
        return;
      }

      let formData = new FormData();
      formData.append("nombre", this.nuevoProducto.nombre);
      formData.append("imagen", this.selectedFile);
      formData.append("descripcion", this.nuevoProducto.descripcion);
      formData.append("precio", this.nuevoProducto.precio);
      formData.append("categoria_id", this.nuevoProducto.categoria_id);
      formData.append("estado", this.nuevoProducto.estado ? 1 : 0);
      formData.append("id_proveedor", this.nuevoProducto.id_proveedor);
      formData.append("iva", this.nuevoProducto.iva);

      try {
        const response = await fetch(`${this.base_url}/productos`, {
          method: "POST",
          body: formData,
        });

        if (!response.ok) {
          throw new Error(`Error en la solicitud: ${response.status}`);
        }

        const productoCreado = await response.json();
        console.log(productoCreado);

        this.productoCreadoCorrectamente = true;
        this.resetForm();
      } catch (error) {
        console.error("Error al crear el producto:", error);
      }
    },

    resetForm() {
      this.nuevoProducto = {
        nombre: "",
        descripcion: "",
        precio: "",
        categoria_id: "",
        estado: false,
        id_proveedor: "",
        iva: "",
      };
      this.selectedFile = null;
    },
  },
  async mounted() {
    console.log("Component mounted, fetching data...");
    await this.fetchCategorias();
    await this.fetchProveedores();
  },
};
</script>

<style scoped>
img {
  max-width: 100%;
  height: auto;
}
</style>
