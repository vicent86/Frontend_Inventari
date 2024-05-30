<template>
    <div class="container">
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
        <tbody>
          <tr>
            <td>{{ cliente.data.nombre }}</td>
            <td>{{ cliente.data.direccion }}</td>
            <td>{{ cliente.data.telefono }}</td>
            <td>{{ cliente.data.cif }}</td>
            <td>{{ cliente.data.estado }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
  export default {
    name: 'ClientesView',
    data() {
      return {
        msg: "Cliente",
        cliente: {
          data: {
            id: "",
            nombre: "",
            direccion: "",
            telefono: "",
            cif: "",
            estado: "",
          }
        },
      };
    },
    props: {
      base_url: String,
    },
    mounted() {
      // El evento mounted se dispara cuando el componente se ha montado en el DOM
      // Hacemos una llamada a una API o realizamos otras operaciones aquí
      this.id = this.$route.params.id;
      console.log(this.id);
      this.getCliente(this.id); // Cambiado a getCliente
    },
    methods: {
      async getCliente(id) { // Cambiado a getCliente
        try {
          let url = this.base_url;
          console.log(url);
          const response = await fetch(url + '/clientes/' + id); // Sin espacio adicional
          if (!response.ok) {
            throw new Error(`HTTP error! Status: ${response.status}`);
          }
          this.cliente = await response.json();
          console.log("El cliente es: ", this.cliente);
        } catch (error) {
          console.error(error);
        }
      },
    },
  };
  </script>
  
  <style scoped></style>
  