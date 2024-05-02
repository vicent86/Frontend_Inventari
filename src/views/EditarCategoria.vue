<template>
    <h1>Editar Categoria</h1>
    <div class="container mt-3">
        <h2>Editar Element</h2>
        <form @submit.prevent="editarCategoria">
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
                <input v-model="nuevaCategoria.estado" type="checkbox" class="form-check-input" id="estado" >

                <span class="badge" :class="{ 'bg-success': nuevaCategoria.estado, 'bg-danger': !nuevaCategoria.estado }">
                  {{ nuevaCategoria.estado ? 'Activo' : 'Inactivo' }}
                </span>
            </div>
            
            <button type="submit" class="btn btn-primary" @click="this.putCategoria()">Actualizar Categoria</button>
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
        id: '',
        nombre: '',
        descripcion: '',
        estado: false,
      },
      categoriaCreadaCorrecta: false
    };
  }, 

  props: {
    base_url: String,
  },
  methods: {
    async editarCategoria() {
      console.log("Categoria editada:", this.nuevaCategoria);
      this.nuevaCategoria.id = this.id;
      await this.putCategoria(this.nuevaCategoria);
      this.categoriaCreadaCorrecta = true;

      // this.nuevaCategoria = {
      //   nombre: "",
      //   descripcion: "",
      //   estado: false
      // };
    },

    async putCategoria(categoria) {
      try {
        const response = await fetch(`${this.base_url}/${this.id}`, {
          method: 'PUT',
          body: JSON.stringify(categoria),
          headers: { 
            'Content-Type': 'application/json; charset=UTF-8' 
          },
        });

        const categoriaCreada = await response.json();
        console.log(categoriaCreada);
      } catch (error) {
        console.error(error);
      }
    },

    async getCategoria() {
      try {
        const response = await fetch(`${this.base_url}/${this.id}`);
        this.nuevaCategoria = await response.json();
        
      } catch (error) {
        console.log(error);
      }
      
    }
  },
  mounted() {
    this.id = this.$route.params.id;
    console.log("ID de la categoria", this.id);
    this.getCategoria();
  }
};
</script>

<style scoped></style>