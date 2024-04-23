<template>
    <h1>Editar Categoria</h1>
    <div class="container mt-3">
        <h2>Editar Element</h2>
        <form @submit.prevent="crearCategoria" >
            <div class="form-group">
                <label for="nombre">Nombre:</label>
                <input v-model="nuevaCategoria.nombre" type="text" class="form-control" id="nombre"
                    placeholder="Introdueix el nom de la categoria">
            </div>

            <div class="form-group">
                <label for="descripcion">Descripcion:</label>
                <textarea v-model="nuevaCategoria.descripcion" class="form-control" id="descripcion"
                    placeholder="Introdueix la descripcio"></textarea>

            </div>

            <div class="form-group">
                <label for="estado">Estado:</label>
                <input v-model="nuevaCategoria.estado" type="checkbox" class="form-check-input" id="estado"  :checked="nuevaCategoria.estado">
            </div>
            <button type="submit" class="btn btn-primary" @click="this.putCategoria()">Añadir Categoria</button>
            <div class="alert alert-success" role="alert" v-if="categoriaCreadaCorrecta">Categoria Editada Correctamente</div>
        </form>
    </div>
</template>

<script>
export default {
  name: 'EditarCategoria',
  data() {
    return {
      nuevaCategoria: {
        nombre: '',
        descripcion: '',
        estado: 'Activo'
      },
      categoriaCreadaCorrecta: false
    };
  }, 

  props: {
    base_url: String,
  },
  methods: {
    editarCategoria() {
      console.log("Categoria añadida:", this.nuevaCategoria);
      this.postCategoria(this.nuevaCategoria);
      this.categoriaCreadaCorrecta = true;

      this.nuevaCategoria = {
        nombre: "",
        descripcion: "",
        estado: 'Activo'
      };
    },

    async putCategoria(category) {
      try {
        const response = await fetch(`http://localhost:8080/categories/${category.id}`, {
          method: 'PUT',
          body: JSON.stringify(category),
          headers: { 'Content-Type': 'application/json; charset=UTF-8' },
        });

        const categoriaCreada = await response.json();
        console.log(categoriaCreada);
      } catch (error) {
        console.error(error);
      }
    },

    async getCategoria() {
      try {
        const response = await fetch(`http://localhost:8080/categories/${this.id}`);
        this.nuevaCategoria = await response.json();
      } catch (error) {
        console.log(error);
      }
    }
  },
  mounted() {
    this.id = this.$route.params.id;
    console.log(this.id);
    this.getCategoria();
  }
};
</script>

<style scoped></style>