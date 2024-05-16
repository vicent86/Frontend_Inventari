<template>
  <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/categories">Categories</router-link> |
    <router-link to="/proveedores">Proveedores</router-link> |
    <router-link to="/productos">Productos</router-link>
  </nav>
  <router-view :base_url="base_url"/>
</template>

<script>
export default {
  data() {
    return {
      base_url: "http://localhost:8000/api",
    };
  },
  methods: {
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
