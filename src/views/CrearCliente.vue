<template>
    <h1>Crear Cliente</h1>
    <div class="container mt-3">
      <h2>Nuevo Cliente</h2>
      <form @submit.prevent="crearCliente">
        <div class="form-group">
          <label for="nombre">Nombre:</label>
          <input
            v-model="nuevoCliente.nombre"
            type="text"
            class="form-control"
            id="nombre"
            placeholder="Introduce el nombre del cliente"
          />
        </div>
  
        <div class="form-group">
          <label for="direccion">Dirección:</label>
          <input
            v-model="nuevoCliente.direccion"
            type="text"
            class="form-control"
            id="direccion"
            placeholder="Introduce la dirección del cliente"
          />
        </div>
  
        <div class="form-group">
          <label for="telefono">Teléfono:</label>
          <input
            v-model="nuevoCliente.telefono"
            type="tel"
            class="form-control"
            id="telefono"
            maxlength="9"
            pattern="\d{9}"
            placeholder="Introduce el teléfono del cliente"
            required
          />
          <small class="form-text text-muted">El teléfono debe tener 9 dígitos.</small>
        </div>
  
        <div class="form-group">
          <label for="cif">CIF:</label>
          <input
            v-model="nuevoCliente.cif"
            type="text"
            class="form-control"
            id="cif"
            placeholder="Introduce el CIF del cliente"
          />
        </div>
  
        <div class="form-group">
          <label for="estado">Estado:</label>
          <input
            v-model="nuevoCliente.estado"
            type="checkbox"
            class="form-check-input"
            id="estado"
          />
        </div>
        <button type="submit" class="btn btn-primary">Añadir Cliente</button>
        <div
          class="alert alert-success"
          role="alert"
          v-if="clienteCreadoCorrectamente"
        >
          Cliente creado correctamente
        </div>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    name: "CrearCliente",
    data() {
      return {
        nuevoCliente: {
          id: 999999,
          nombre: "",
          direccion: "",
          telefono: "",
          cif: "",
          estado: false,
        },
        clienteCreadoCorrectamente: false,
      };
    },
  
    props: {
      base_url: String,
    },
    methods: {
      async crearCliente() {
        console.log("Cliente añadido:", this.nuevoCliente);
        // Lógica para crear la categoría
        this.nuevoCliente.id = Math.floor(Math.random() * 1000000);
        await this.postCliente(this.nuevoCliente);
        this.clienteCreadoCorrectamente = true;
  
        this.nuevoCliente = {
          id: "",
          nombre: "",
          direccion: "",
          telefono: "",
          cif: "",
          estado: false,
        };
      },
      async postCliente(cliente) {
        try {
          const response = await fetch(`${this.base_url}/clientes`, {
            method: "POST",
            body: JSON.stringify(cliente),
            headers: {
              "Content-Type": "application/json; charset=UTF-8",
            },
          });
  
          const clienteCreado = await response.json();
          console.log(clienteCreado);
        } catch (error) {
          console.error(error);
        }
      },
    },
    mounted() {},
  };
  </script>
  