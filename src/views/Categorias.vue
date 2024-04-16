<template>
    <div>
        <h1>{{ msg }}</h1>
    </div>
    <div class="container">
        <button type="button" class="btn btn-outline-info" @click="this.editarCategoria()">Añadir Categoria</button>
        <br>
        <table class="table table-striped" v-for="category in categories.data" :key="category.id">
            <thead>
                <tr>
                    <th>Nombre</th>
                    <th>Descripcion</th>
                    <th>Estado</th>
                </tr>
            </thead>
            <tbody>
                <tr >
                    <td>{{ category.nombre }}</td>
                    <td>{{ category.descripcion }}</td>
                    <td>
                        <span class="badge bg-success" v-if="category.estado == 'Activo'">Activo</span> 
                        <span class="badge bg-danger" v-else>Inactivo</span>
                    </td>
                    <td>
                        <button class="btn btn-outline-info" @click="this.VistaCategoria(category.id)">Vista</button>
                        <button class="btn btn-outline-warning" @click="this.editarCategoria(category.id)">Editar</button>
                        <button class="btn btn-outline-danger" @click="this.borrarCategoria(category.id)"><a  href="https://icons8.com/icon/67466/papelera-llena">Papelera llena</a> icon by <a href="https://icons8.com">Icons8</a> Borrar</button>
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
            categories: [],
        };
    },
    props: {
        base_url: String,
    },
    methods:{
        async getCategories() {
            try {
                const response = await fetch('http://localhost:8080/categories');
                this.categories = await response.json();
            } catch (error) {
                console.error(error);
            }
        },
        async borrarCategoria(id){
            try {
                await fetch(`http://localhost:8080/categories/${id}`,{
                    method:'DELETE'
                });
                await this.getCategories();
            } catch (error) {
                console.error(error);
            }
        },
        nuevaCategoria(){
            this.$router.push('nuevaCategoria')
        },
        VistaCategoria(identity) {
            this.$router.push({ name:'category', params: { id : identity }});
        },
        editarCategoria(identity) {
            this.$router.push({name:'category',params: { id : identity }});
        }
    },

    mounted() {
        this.getCategories();
        this.borrarCategoria();
    }

}
</script>

<style scoped></style>