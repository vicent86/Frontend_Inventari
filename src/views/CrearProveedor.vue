<template>
    <h1>Crear Proveedor</h1>
    <div class="container mt-3">
        <h2>Nuevo Proveedor</h2>
        <form @submit.prevent="crearProveedor" >
            <div class="form-group">
                <label for="nombre">Nombre:</label>
                <input v-model="nuevoProveedor.nombre" type="text" class="form-control" id="nombre"
                    placeholder="Introdueix el nom del proveedor">
            </div>

            <div class="form-group">
                <label for="direccion">Direccion:</label>
                <input v-model="nuevoProveedor.direccion" type="text" class="form-control" id="direccion"
                    placeholder="Introdueix la direccio del proveedor">
            </div>

            <div class="form-group">
                <label for="email">Email:</label>
                <div class="input-group mb-3">
                    <span class="input-group-text" id="basic-addon1">@</span>
                    <input v-model="nuevoProveedor.email" type="text" class="form-control" placeholder="Introdueix el correu del proveedor" aria-label="Correu" aria-describedby="basic-addon1">
                </div>
            </div>

            <div class="form-group">
                <label for="cif">CIF:</label>
                <input v-model="nuevoProveedor.cif" type="text" class="form-control" id="cif"
                    placeholder="Introdueix el CIF del proveedor">
            </div>

            <div class="form-group">
                <label for="estado">Estado:</label>
                <input v-model="nuevoProveedor.estado" type="checkbox" class="form-check-input" id="estado">
            </div>
            <div class="form-group">
                <label for="cualificacion">Cualificación:</label>
                <div class="star-rating">
                <span 
                    v-for="star in 5" 
                    :key="star" 
                    :class="{ 'filled': star <= nuevoProveedor.cualificacion }"
                    @mouseover="hoverRating(star)"
                    @click="setRating(star)"
                >
                    ★
                </span>
                </div>
                <input v-model="nuevoProveedor.cualificacion" type="hidden"  id="cualificacion" />
                <span class="text-danger">{{ nuevoProveedor.errorMessage }}</span>
            </div>

            <button type="submit" class="btn btn-primary">Añadir Proveedor</button>
            <div class="alert alert-success" role="alert" v-if="proveedorCreatCorrecte">Proveedor creado correctamente</div>
        </form>
    </div>
</template>

<script>
export default {
  name: 'CrearProveedor',
  data() {
    return {
      nuevoProveedor: {
        id: 999999,
        nombre: '',
        direccion: '',
        email: '',
        cif:'',
        estado: false,
        cualificacion: null,
        hoveredRating: -1, 
        selectedRating: -1,
        errorMessage: '',
      },
      proveedorCreatCorrecte: false
    };
  },

  props: {
    base_url: String,
  },
  methods: {
    async crearProveedor() {
      console.log("Proveedor añadido:", this.nuevoProveedor);
      // Lógica para crear la categoría
      this.nuevoProveedor.id = Math.floor(Math.random() * 1000000);
      this.postProveedor(this.nuevoProveedor);
      this. proveedorCreatCorrecte = true;

      this.nuevoProveedor = {
        id: "",
        nombre: "",
        direccion: "",
        email: "",
        cif:"",
        estado: false,
        cualificacion: Number,
        hoveredRating: -1, 
        selectedRating: -1,
        errorMessage: "",
      };
      
    },
    async postProveedor(proveedor) {
      try {
        const response = await fetch(`${this.base_url}/proveedores`, {
          method: 'POST',
          body: JSON.stringify(proveedor),
          headers: { 
            "Content-Type": "application/json; charset=UTF-8" 
          },
        });
       
        const proveedorCreado = await response.json();
        console.log(proveedorCreado);
      } catch (error) {
        console.error(error);
      }
    },

    async hoverRating(rating) {
      this.nuevoProveedor.errorMessage = '';
      this.nuevoProveedor.cualificacion = rating;
    },
    async setRating(rating) {
      this.nuevoProveedor.cualificacion = rating;
    }
  },
  mounted() {

  }
};
</script>

<style scoped>
    .star-rating {
        unicode-bidi: bidi-override;
        direction: rtl;
    }
    .star-rating span {
        cursor: pointer;
    }
    .star-rating span:hover,
    .star-rating span:hover ~ span {
        color: orange;
    }
    .star-rating .filled {
        color: orange;
    }
</style>