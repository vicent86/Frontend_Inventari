<template>
    <h1>Crear Categoria</h1>
    <div class="container mt-3">
        <h2>nueva Categoria</h2>
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
                <input v-model="nuevaCategoria.estado" type="checkbox" class="form-check-input" id="estado">
            </div>
            <button type="submit" class="btn btn-primary">Añadir Categoria</button>
            <div class="alert alert-success" role="alert" v-if="categoriaCreadaCorrecta">Categoria creada correctamente</div>
        </form>
    </div>
</template>

<script>
export default {
  name: 'CrearCategoria',
  data() {
    return {
      nuevaCategoria: {
        id: 999999,
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
    async crearCategoria() {
      console.log("Categoria añadida:", this.nuevaCategoria);
      // Lógica para crear la categoría
      this.nuevaCategoria.id = Math.floor(Math.random() * 1000000);
      this.postCategoria(this.nuevaCategoria);
      this.categoriaCreadaCorrecta = true;

      this.nuevaCategoria = {
        id: "",
        nombre: "",
        descripcion: "",
        estado: false,
      };
      
    },
    async postCategoria(categoria) {
      try {
        const response = await fetch(`${this.base_url}/categorias`, {
          method: 'POST',
          body: JSON.stringify(categoria),
          headers: { 
            "Content-Type": "application/json; charset=UTF-8" 
          },
        });
       
        const categoriaCreada = await response.json();
        console.log(categoriaCreada);
      } catch (error) {
        console.error(error);
      }
    }
  },
  mounted() {

  }
};
</script>
