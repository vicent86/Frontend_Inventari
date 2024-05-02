<template>
  <div>
    <h1>{{ msg }}</h1>
  </div>
  <div class="container">
    <button
      type="button"
      class="btn btn-outline-info"
      @click="this.nuevaCategoria(categoriaIndex)"
    >
      Añadir Categoria
    </button>
    <br />
    <table class="table table-striped">
      <thead>
        <tr>
          <th>Nombre</th>
          <th>Descripcion</th>
          <th>Estado</th>
        </tr>
      </thead>
      <tbody v-for="category in categories.data" :key="category.id">
        <tr>
          <td>{{ category.nombre }}</td>
          <td>{{ category.descripcion }}</td>
          <td>
            <span class="badge bg-success" v-if="category.estado">Activo</span>
            <span class="badge bg-danger" v-else>Inactivo</span>
          </td>
          <td>
            <button
              class="btn btn-outline-info"
              @click="this.VistaCategoria(category.id)"
            >
              Vista
            </button>
            <button
              class="btn btn-outline-warning"
              @click="this.editarCategoria(category.id)"
            >
              Editar
            </button>
            <button
              class="btn btn-outline-danger"
              @click="this.borrarCategoria(category.id)"
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

export  default {

    name: 'EditarCategoria',
    data() {
        return {
            msg: "Lista de Categorias",
            id: 1,
            categoria: {
              id: 1,
              nombre: 'Informatica y Accesorios',
              descripcion: 'Accesorios y equipos informaticos en general',
              estado: 'Activo',
            },
            categories: { data: [] },
        };
    },
    props: {
      base_url: String,
    },
    methods:{
        async getCategories() {
            try {
                let url = this.base_url;
                console.log(url);
                const response = await fetch(url);
                this.categories = await response.json();
                console.log("Les categories son:  ",this.categories);
            } catch (error) {
                console.error(error);
            }
        },
        async borrarCategoria(id){
            try {
                await fetch(`${this.base_url}/${id}`, {
                    method: 'DELETE',
                });
                await this.getCategories();
            } catch (error) {
                console.error(error);
            }
        },
        async nuevaCategoria(categoriaIndex){

          this.$router.push({ name:'nuevacategoria', params: { id : categoriaIndex }});
           
        },
        async VistaCategoria(id) {
          this.$router.push({ name:'categoria', params: { id : id }});
        },
        async editarCategoria(id) {
          this.$router.push({name:'editarcategoria', params: { id : id }});
        },
  
    },

    mounted() {
      this.id = this.$route.params.id;
      //console.log("ID de la categoria a mostrar es " + this.id);
      this.getCategories();
        
    }

}
</script>

<style scoped></style>
