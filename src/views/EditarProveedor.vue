<template>
  <h1>Editar Proveedor</h1>
  <div class="container mt-3">
    <h2>Editar Element</h2>
    <form @submit.prevent="editarProveedor">
      <div class="form-group">
        <label for="nombre">Nombre:</label>
        <input
          v-model="nuevoProveedor.data.nombre"
          type="text"
          class="form-control"
          id="nombre"
          placeholder="Introdueix el nom del Proveedor"
        />
      </div>

      <div class="form-group">
        <label for="direccion">Direccion:</label>
        <input
          v-model="nuevoProveedor.data.direccion"
          type="text"
          class="form-control"
          id="direccion"
          placeholder="Introdueix la Direccio del Proveedor"
        />
      </div>

      <div class="form-group">
        <label for="email">Email:</label>
        <div class="input-group mb-3">
          <span class="input-group-text" id="basic-addon1">@</span>
          <input
            v-model="nuevoProveedor.data.email"
            type="text"
            class="form-control"
            placeholder="Introdueix el correu del proveedor"
            aria-label="Correu"
            aria-describedby="basic-addon1"
          />
        </div>
      </div>

      <div class="form-group">
        <label for="cif">CIF:</label>
        <input
          v-model="nuevoProveedor.data.cif"
          type="text"
          class="form-control"
          id="cif"
          placeholder="Introdueix el CIF del Proveedor"
        />
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
            'bg-success': nuevoProveedor.data.estado,
            'bg-danger': !nuevoProveedor.data.estado,
          }"
        >
          {{ nuevoProveedor.data.estado ? "Activo" : "Inactivo" }}
        </span>
      </div>

      <div class="form-group">
        <label for="cualificacion">Cualificación</label>
        <div class="star-rating">
          <span
            v-for="star in 5"
            :key="star"
            :class="{ filled: star <= nuevoProveedor.data.cualificacion }"
            @mouseover="hoverRating(star)"
            @click="setRating(star)"
          >
            ★
          </span>
        </div>
        <input
          v-model="nuevoProveedor.data.cualificacion"
          type="hidden"
          id="cualificacion"
        />
        <span class="text-danger">{{ nuevoProveedor.data.errorMessage }}</span>
      </div>

      <button type="submit" class="btn btn-primary">
        Actualizar Proveedor
      </button>
      <div
        class="alert alert-success"
        role="alert"
        v-if="proveedorCreatCorrecte"
      >
        Proveeedor Editado Correctamente
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "EditarProveedor",
  data() {
    return {
      nuevoProveedor: {
        data: {
          id: "",
          nombre: "",
          direccion: "",
          email: "",
          cif: "",
          estado: false,
          cualificacion: Number,
          errorMessage: "",
        },
      },
      proveedorCreatCorrecte: false,
    };
  },

  props: {
    base_url: String,
  },
  methods: {
    async editarProveedor() {
      try {
        const response = await fetch(
          `${this.base_url}/proveedores/${this.nuevoProveedor.data.id}`,
          {
            method: "PUT",
            headers: {
              "Content-Type": "application/json; charset=UTF-8",
            },
            body: JSON.stringify(this.nuevoProveedor.data),
          }
        );

        if (!response.ok) {
          throw new Error(`HTTP error! Status: ${response.status}`);
        }

        this.proveedorCreatCorrecte = true;
      } catch (error) {
        console.error("Error al actualizar el proveedor:", error.message);
      }
    },

    async getProveedor(id) {
      try {
        const url = `${this.base_url}/proveedores/${id}`;
        const response = await fetch(url);
        if (!response.ok) {
          throw new Error(`HTTP error! Status: ${response.status}`);
        }
        const data = await response.json(); 
        this.nuevoProveedor.data = data;
      } catch (error) {
        console.error("Error al obtener el proveedor:", error);
      }
    },

    toggleEstado() {
      this.nuevoProveedor.data.estado = !this.nuevoProveedor.data.estado;
    },

    async hoverRating(rating) {
      this.nuevoProveedor.data.errorMessage = "";
      this.nuevoProveedor.data.cualificacion = rating;
    },
    async setRating(rating) {
      this.nuevoProveedor.data.cualificacion = rating;
    },
  },
  mounted() {
    this.getProveedor(this.$route.params.id);
  },
};
</script>

<style scoped>
.star-rating {
  font-size: 25px;
}
.star-rating span {
  cursor: pointer;
  color: #ccc;
}
.star-rating span.filled {
  color: orange;
}
</style>
