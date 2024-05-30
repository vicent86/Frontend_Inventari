<template>
  <div>
    <h1>{{ msg }}</h1>
  </div>
  <div class="container">
    <button
      type="button"
      class="btn btn-outline-info"
      @click="this.nuevoCliente(clienteIndex)"
    >
      Añadir Cliente
    </button> |
    <button type="button" class="btn btn-outline-secondary" @click="generarPDF">
      Generar PDF
    </button>
    <br />
    <table class="table table-striped">
      <thead>
        <tr>
          <th>Nombre</th>
          <th>Direccion</th>
          <th>Telefono</th>
          <th>CIF</th>
          <th>Estado</th>
        </tr>
      </thead>
      <tbody v-for="cliente in clientes.data" :key="cliente.id">
        <tr>
          <td>{{ cliente.nombre }}</td>
          <td>{{ cliente.direccion }}</td>
          <td>{{ cliente.telefono }}</td>
          <td>{{ cliente.cif }}</td>
          <td>
            <span class="badge bg-success" v-if="cliente.estado === 'Activo'"
              >Activo</span
            >
            <span class="badge bg-danger" v-else>Inactivo</span>
          </td>
          <td>
            <button
              class="btn btn-outline-info"
              @click="this.VistaCliente(cliente.id)"
            >
              Vista
            </button>
            <button
              class="btn btn-outline-warning"
              @click="this.editarCliente(cliente.id)"
            >
              Editar
            </button>
            <button
              class="btn btn-outline-danger"
              @click="this.borrarCliente(cliente.id)"
            >
              Borrar
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { jsPDF } from "jspdf";

export default {
  name: "ClientesView",
  data() {
    return {
      msg: "Lista de Clientes",
      id: 1,
      cliente: {
        id: 1,
        nombre: "Pepe Pozas Garcia",
        direccion: "Av. Los palacios, 56 Manises",
        telefono: "615 123 456",
        cif: "B-12345678",
        estado: "Activo",
      },
      clientes: { data: [] },
    };
  },
  props: {
    base_url: String,
  },
  methods: {
    async getClientes() {
      try {
        let url = this.base_url + "/clientes";
        console.log(url);
        const response = await fetch(url);
        this.clientes = await response.json();
        console.log("Els clients son:  ", this.clientes);
      } catch (error) {
        console.error(error);
      }
    },
    async borrarCliente(id) {
      try {
        const confirmacion = confirm(
          "¿Estás seguro de que deseas borrar este cliente?"
        );
        if (confirmacion) {
          await fetch(`${this.base_url}/clientes/${id}`, {
            method: "DELETE",
          });

          await this.getClientes();
          alert("Cliente Borrado Correctamente");
        }
      } catch (error) {
        console.error(error);
      }
    },
    async nuevoCliente(clienteIndex) {
      this.$router.push({ name: "nuevocliente", params: { id: clienteIndex } });
    },
    async VistaCliente(id) {
      this.$router.push({ name: "cliente", params: { id: id } });
    },
    async editarCliente(id) {
      this.$router.push({ name: "editarcliente", params: { id: id } });
    },
    async actualizarEstadoCliente(id, estado) {
      const index = this.clientes.data.findIndex((c) => c.id === id);
      if (index !== -1) {
        this.clientes.data[index].estado = estado;
      }
    },
    generarPDF() {
      const doc = new jsPDF();
      doc.text("Lista de Clientes", 20, 10);
      const clientes = this.clientes.data;
      let y = 20;
      const pageHeight = doc.internal.pageSize.height;
      const lineHeight = 10;
      const margin = 20;

      clientes.forEach((cliente, index) => {
        // Comprobar si hay suficiente espacio en la página actual
        if (y + lineHeight * 6 > pageHeight - margin) {
          doc.addPage();
          y = margin; // Reiniciar el y para la nueva página
        }

        doc.text(`Cliente ${index + 1}`, 10, y);
        doc.text(`Nombre: ${cliente.nombre}`, 10, y + lineHeight);
        doc.text(`Direccion: ${cliente.direccion}`, 10, y + lineHeight * 2);
        doc.text(`Telefono: ${cliente.telefono}`, 10, y + lineHeight * 3);
        doc.text(`CIF: ${cliente.cif}`, 10, y + lineHeight * 4);
        doc.text(`Estado: ${cliente.estado}`, 10, y + lineHeight * 5);
        y += lineHeight * 6; // Incrementa y para el siguiente cliente
      });

      doc.save("lista_de_clientes.pdf");
    },
  },

  mounted() {
    this.id = this.$route.params.id;
    this.getClientes();
  },
};
</script>

<style scoped></style>
