<template>
 <div class="container">
  <table class="table table-striped" >
    <thead>
      <tr>
        <th>Nombre</th>
        <th>Descripcion</th>
        <th>Estado</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>{{ categoria.data.nombre }}</td>
        <td>{{ categoria.data.descripcion }}</td>
        <td>{{ categoria.data.estado }}</td>
      </tr>
    </tbody>
  </table>
 </div>
</template>

<script>
export default {
  name: 'CategoriesView',
  data() {
    return {
      msg: "Categoria",
      categoria: {
        data: {
          id: "",
          nombre: "",
          descripcion: "",
          estado: "",
        }
      },
    };
  },
  props:{
    base_url: String,
  },
  mounted() {
    // L'esdeveniment mounted es dispara quan el component s'ha muntat al DOM
    // Fem una crida a una API o realitzem altres operacions aquí
    this.id = this.$route.params.id;
    console.log(this.id);
    this.getCategories(this.id);
  },
  methods: {
    async getCategories(id) {
      try {
        let url= this.base_url;
        console.log(url);
        const response = await fetch(url + '/categorias/' + id);
        this.categoria = await response.json();
        console.log("Les categories són: " + this.categoria);
      } catch (error) {
        console.error(error);
      }
    },
  },
};

</script>

<style scoped></style>