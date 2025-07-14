<template>
  <div class="app">
    <h1>📚 Explorador de Libros</h1>
    <SearchForm @add-fav="addFavorite" />
    <FavoriteList :books="favorites" @update="updateFavorites" />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import SearchForm from './components/SearchForm.vue';
import FavoriteList from './components/FavoriteList.vue';
import { getFavorites, saveFavorites } from './services/bookService.js';

const favorites = ref([]);

onMounted(() => {
  favorites.value = getFavorites();
});

function addFavorite(book) {
  if (!favorites.value.some(b => b.key === book.key)) {
    favorites.value.push(book);
    saveFavorites(favorites.value);
  }
}

function updateFavorites(newList) {
  favorites.value = newList;
  saveFavorites(favorites.value);
}
</script>

<style>
.app {
  font-family: Arial, sans-serif;
  padding: 2rem;
  max-width: 800px;
  margin: auto;
}
</style>