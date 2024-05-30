<template>
  <div class="cover">
    <h1>{{ title }}</h1>
    <img src="@/assets/OIP.jpeg" alt="Inventory" class="central-image" />
    <p>{{ description }}</p>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      title: 'Sistema de Inventario',
      description: 'Bienvenido al sistema de inventario. Aquí puedes gestionar tus productos, categorías, proveedores y clientes de manera eficiente.'
    }
  }
}
</script>

<style scoped>
.cover {
  text-align: center;
  margin-top: 50px;
}
.central-image {
  width: 50%;
  margin: 20px auto;
  display: block;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}
h1 {
  font-size: 2.5rem;
  color: #42b983;
  margin-bottom: 20px;
}
p {
  font-size: 1.2rem;
  color: #666;
  margin: 20px 0;
}
</style>
