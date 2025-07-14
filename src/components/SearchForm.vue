<template>
  <form @submit.prevent="searchBooks">
    <input v-model="query" placeholder="Buscar libro por título..." required />
    <button type="submit">Buscar</button>

    <ul v-if="results.length">
      <li v-for="book in results" :key="book.key">
        <img
          v-if="book.cover_i"
          :src="'https://covers.openlibrary.org/b/id/' + book.cover_i + '-S.jpg'"
          alt="cover"
        />
        <div>
          <strong>{{ book.title }}</strong><br />
          <em>{{ book.author_name?.[0] || 'Autor desconocido' }}</em><br />
          <button @click="add(book)">Agregar a favoritos</button>
        </div>
      </li>
    </ul>
  </form>
</template>

<script setup>
import { ref } from 'vue';

const emit = defineEmits(['add-fav']);
const query = ref('');
const results = ref([]);

async function searchBooks() {
  const res = await fetch(`https://openlibrary.org/search.json?q=${encodeURIComponent(query.value)}`);
  const data = await res.json();
  results.value = data.docs.slice(0, 5);
}

function add(book) {
  emit('add-fav', book);
}
</script>

<style scoped>
form {
  margin-bottom: 2rem;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  margin-bottom: 1rem;
  display: flex;
  gap: 1rem;
}
img {
  height: 80px;
}
</style>