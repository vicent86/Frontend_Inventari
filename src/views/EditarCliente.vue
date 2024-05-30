<template>
    <h1>Editar Cliente</h1>
    <div class="container mt-3">
      <h2>Editar Element</h2>
      <form @submit.prevent="editarCliente">
        <div class="form-group">
          <label for="nombre">Nombre:</label>
          <input
            v-model="nuevoCliente.data.nombre"
            type="text"
            class="form-control"
            id="nombre"
            placeholder="Introdueix el nom de la categoria"
          />
        </div>
  
        <div class="form-group">
        <label for="direccion">Direccion:</label>
        <input
          v-model="nuevoCliente.data.direccion"
          type="text"
          class="form-control"
          id="direccion"
          placeholder="Introdueix la direcció del client"
        />
      </div>

      <div class="form-group">
        <label for="telefono">Teléfono:</label>
        <input
          v-model="nuevoCliente.data.telefono"
          type="tel"
          class="form-control"
          id="telefono"
          maxlength="9"
          pattern="\d{9}"
          placeholder="Introdeix el telèfon del client"
          required
        />
        <small class="form-text text-muted">El telèfon deu de tindre 9 dígits.</small>
      </div>

      <div class="form-group">
        <label for="cif">CIF:</label>
        <input
          v-model="nuevoCliente.data.cif"
          type="text"
          class="form-control"
          id="cif"
          placeholder="Introdueix el cif del client"
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
              'bg-success': nuevoCliente.data.estado,
              'bg-danger': !nuevoCliente.data.estado,
            }"
          >
            {{ nuevoCliente.data.estado ? "Activo" : "Inactivo" }}
          </span>
        </div>
  
        <button type="submit" class="btn btn-primary">
          Actualizar Cliente
        </button>
        <div
          class="alert alert-success"
          role="alert"
          v-if="clienteCreatCorrecte"
        >
          Cliente Editado Correctamente
        </div>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    name: "EditarCliente",
    data() {
      return {
        nuevoCliente: {
          data: {
            id: "",
            nombre: "",
            direccion: "",
            telefono: "",
            cif: "",
            estado: false,
          },
        },
        clienteCreatCorrecte: false,
      };
    },
  
    props: {
      base_url: String,
    },
    methods: {
      async editarCliente() {
        try {
          let response = await fetch(
            `${this.base_url}/clientes/${this.nuevoCliente.data.id}`,
            {
              method: "PUT",
              headers: {
                "Content-Type": "application/json; charset=UTF-8",
              },
              body: JSON.stringify(this.nuevoCliente.data),
            }
          );
  
          if (!response.ok) {
            throw new Error(`HTTP error! Status: ${response.status}`);
          }
  
          this.clienteCreatCorrecte = true;
        } catch (error) {
          console.error("Error al actualizar el cliente:", error.message);
        }
      },
  
      async getCliente(id) {
        try {
          const url = `${this.base_url}/clientes/${id}`;
          const response = await fetch(url);
          if (!response.ok) {
            throw new Error(`HTTP error! Status: ${response.status}`);
          }
          this.nuevoCliente = await response.json();
        } catch (error) {
          console.error("Error al obtener el cliente:", error);
        }
      },
  
      toggleEstado() {
        this.nuevoCliente.data.estado = !this.nuevoCliente.data.estado;
      }
    },
    mounted() {
      
      this.getCliente(this.$route.params.id);
    },
  };
  </script>
  
  <style scoped></style>
  