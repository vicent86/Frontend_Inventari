<template>
 <div class="container">
  <table class="table table-striped" v-for="category in categories.data" :key="category.id">
    <thead>
      <tr>
        <th>Nombre</th>
        <th>Descripcion</th>
        <th>Estado</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>{{ category.nombre }}</td>
        <td>{{ category.descripcion }}</td>
        <td>{{ category.estado }}</td>
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
      msg: "Lista de Categorias",
      categories: [],
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
    this.getCategories();
  },
  methods: {
    async getCategories() {
      try {
        let url= this.base_url;
        console.log(url);
        const response = await fetch(url);
        this.categories = await response.json();
        console.log("Les categories són: " + this.categories);
      } catch (error) {
        console.error(error);
      }
    },
  },
};

</script>

<style scoped></style>