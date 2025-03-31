<script setup>
import { RouterLink, RouterView } from 'vue-router'
const navArr = [
  { id: 1, title: 'Home', path: '/' },
  { id: 2, title: 'Panel', path: '/panel' },
  { id: 3, title: 'About', path: '/about' },
]

const toggleTheme = () => {
  document.documentElement.classList.toggle('dark-theme')
}
</script>

<template>
  <header>
    <div class="nav-container">
      <nav>
        <span class="nav-title">Navigation</span>
        <RouterLink v-for="item in navArr" :to="item.path" :key="item.id">
          {{ item.title }}
        </RouterLink>
        <button @click="toggleTheme">Cambiar Tema</button>
      </nav>
    </div>
  </header>

  <main class="container">
    <RouterView />
  </main>

  <footer>
    <h4>Pie de pagina</h4>
  </footer>
</template>

<style scoped>
.nav-container {
  text-align: center;
  justify-content: space-between;
}

nav {
  display: flex;
  align-items: center;
  font-size: 14px;
  padding: 0.5rem;
  border-radius: 8px;
}

.nav-title {
  font-size: 1.5rem;
  font-weight: bold;
  margin-right: 1rem;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  text-decoration: none;
}

nav a:hover {
  text-decoration: underline;
}

/* Estilo para el enlace activo */
.router-link-active {
  font-weight: bold;
  /* Cambia el color del enlace activo */
  text-decoration: underline; /* Opcional: subraya el enlace activo */
}
</style>
