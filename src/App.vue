<script setup>
import { RouterView, RouterLink } from 'vue-router';
</script>

<template>
  <nav class="menu">
    <RouterLink to="/" class="link">Home</RouterLink>
    <RouterLink to="/phone" class="link">Phone</RouterLink>
    <RouterLink to="/movie" class="link">Movie</RouterLink>
    <RouterLink to="/post" class="link">Post</RouterLink>
    <RouterLink to="/tv" class="link">Tv</RouterLink>
  </nav>
  <div class="container">
    <RouterView />
  </div>
</template>

<style scoped>
.menu {
  display: flex;
  justify-content: center; 
  background-color: #001499; 
  padding: 10px 20px;
  border-radius: 8px;
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1); 
  margin-bottom: 20px;
}

.link {
  color: white;
  text-decoration: none;
  font-weight: bold;
  padding: 8px 15px;
  margin: 0 10px;
  border-radius: 5px;
}

.link:hover {
  color: #1a009c;
  background-color: rgb(1, 168, 245);
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.2);
  cursor: pointer;
}
</style>
