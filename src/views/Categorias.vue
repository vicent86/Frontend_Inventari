<template>
  <div>
    <h1>{{ msg }}</h1>
  </div>
  <div class="container">
    <button
      type="button"
      class="btn btn-outline-info"
      @click="this.crearCategoria(id)"
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
            <span class="badge bg-success" v-if="category.estado === 1">Activo</span>
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
              @click="this.editarCategoria(category.id.crearCategoria)"
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
            categories: [] ,
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

            // app.delete('/categories/:id', (req, res) => {
            //     const categoriaId = parseInt(req.params.id);
            //     categories = categories.filter(c => c.id !== categoriaId);
            //     res.json({ msg:"Se ha eliminado la categoría correctamente" });
            // });
        },
        async nuevaCategoria(categoriaIndex){

            try {
                const response = await fetch(`${this.base_url}`, {
                    method: 'POST',
                    headers: {
                        'Content-Type':  'application/json'
                    },
                    body: JSON.stringify(categoriaIndex)
                });

                if (response.status === 201) {
                    const nuevaCategoria = await response.json();
                    console.log('Nueva categoría creada:', nuevaCategoria);
                } else {
                    console.error('Error al crear la categoría');
                }

            } catch (error) {
                console.error(error);
            }
            //this.$router.push('NuevaCategoria')
            // app.post( '/categories', ( req,res )=>{
            //     const nuevaCategoria = req.body
            //     categories.push(nuevaCategoria);
            //     res.status(201).json(nuevaCategoria);
            // });
        },
        async VistaCategoria(id) {

            try {
                const response = await fetch(`${this.base_url}/${id}`, {
                    method: 'GET',
                });
                if (response.status === 404) {
                    console.error('Categoría no encontrada');
                } else {
                    const categoria = await response.json();
                    console.log('Información de la categoría:', categoria);
            
                }

            } catch (error) {
                console.error(error);
            }
            // //this.$router.push({ name:'VistaCategoria', params: { id : id }});
            // app.get('/categories/:id', (req, res)=>{
            //     const categoriaId = parseInt(req.params.id);
            //     const categoria = categories.find(c => c.id === categoriaId );
            //         if(!categoria){
            //             res.status(404).json({msg:'La Categoría no encontrada'});
            //         } else {
            //             res.json(categoria);
            //         }
            // });
        },
        async editarCategoria(id, crearCategoria) {
            //this.$router.push({name:'EditarCategoria',params: { id : id }});
            // app.put('/categories/:id', (req,res)=>{
            //     const categoriaId = parseInt(req.params.id);
            //     const categoriaIndex = categories.findIndex(c => c.id === categoriaId);

            //     if (categoriaIndex === -1 ) {
            //         res.status(404).json({ msg: 'Categoria No Encontrada' })
            //     } else {
            //         categories[categoriaIndex] = req.body;
            //         res.json(categories[categoriaIndex]);
            //     }
            // });

            try {
                const response = await fetch(`${this.base_url}/${id}`, {
                    method: "PUT",
                    headers: {
                        'Content-type': 'application/json',
                    },
                    body: JSON.stringify(crearCategoria)
                });

                if (response.status === 404) {
                    const errorData = await response.json();
                    console.error(errorData.msg);
                } else {
                    const updatedCategoria = await response.json();
                    console.log('Categoría actualizada:', updatedCategoria);
                }
            } catch (error) {
                console.log(error);
            }
        }
    },

    mounted() {
        this.getCategories();
        //this.borrarCategoria();
    }

}
</script>

<style scoped></style>
