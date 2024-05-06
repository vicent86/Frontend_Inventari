<template>
  <h1>Editar Categoria</h1>
  <div class="container mt-3">
    <h2>Editar Element</h2>
    <form @submit.prevent="editarCategoria">
      <div class="form-group">
        <label for="nombre">Nombre:</label>
        <input
          v-model="nuevaCategoria.data.nombre"
          type="text"
          class="form-control"
          id="nombre"
          placeholder="Introdueix el nom de la categoria"
        />
      </div>

      <div class="form-group">
        <label for="descripcion">Descripcion:</label>
        <textarea
          v-model="nuevaCategoria.data.descripcion"
          class="form-control"
          id="descripcion"
          placeholder="Introdueix la descripcio"
        ></textarea>
      </div>

      <div class="form-group">
        <label for="estado">Estado:</label>
        <input
          type="checkbox"
          class="form-check-input"
          id="estado"
          @change="toggleEstado"
        />
        <span
          class="badge"
          :class="{
            'bg-success': nuevaCategoria.data.estado,
            'bg-danger': !nuevaCategoria.data.estado,
          }"
        >
          {{ nuevaCategoria.data.estado ? "Activo" : "Inactivo" }}
        </span>
      </div>

      <button type="submit" class="btn btn-primary">
        Actualizar Categoria
      </button>
      <div
        class="alert alert-success"
        role="alert"
        v-if="categoriaCreadaCorrecta"
      >
        Categoria Editada Correctamente
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "EditarCategoria",
  data() {
    return {
      nuevaCategoria: {
        data: {
          id: "",
          nombre: "",
          descripcion: "",
          estado: false,
        },
      },
      categoriaCreadaCorrecta: false,
    };
  },

  props: {
    base_url: String,
  },
  methods: {
    async editarCategoria() {
      try {
        const response = await fetch(
          `${this.base_url}/categorias/${this.nuevaCategoria.data.id}`,
          {
            method: "PUT",
            headers: {
              "Content-Type": "application/json; charset=UTF-8",
            },
            body: JSON.stringify(this.nuevaCategoria.data),
          }
        );

        if (!response.ok) {
          throw new Error(`HTTP error! Status: ${response.status}`);
        }

        this.categoriaCreadaCorrecta = true;
      } catch (error) {
        console.error("Error al actualizar la categoría:", error.message);
      }
    },

    async getCategoria(id) {
      try {
        const url = `${this.base_url}/categorias/${id}`;
        const response = await fetch(url);
        if (!response.ok) {
          throw new Error(`HTTP error! Status: ${response.status}`);
        }
        this.nuevaCategoria = await response.json();
      } catch (error) {
        console.error("Error al obtener la categoría:", error);
      }
    },

    toggleEstado() {
      this.nuevaCategoria.data.estado = !this.nuevaCategoria.data.estado;
    }
  },
  mounted() {
    // this.id = this.$route.params.id;
    // console.log("ID de la categoria", this.id);
    // this.getCategoria(this.id);
    this.getCategoria(this.$route.params.id);
  },
};
</script>

<style scoped></style>
