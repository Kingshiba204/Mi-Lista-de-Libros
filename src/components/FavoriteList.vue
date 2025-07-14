<template>
  <div>
    <h2>📌 Libros Favoritos</h2>
    <ul>
      <li v-for="book in books" :key="book.key">
        <img
          v-if="book.cover_i"
          :src="'https://covers.openlibrary.org/b/id/' + book.cover_i + '-S.jpg'"
          alt="cover"
        />
        <div>
          <strong>{{ book.title }}</strong><br />
          <em>{{ book.author_name?.[0] || 'Autor desconocido' }}</em><br />
          <button @click="remove(book.key)">Eliminar</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
const props = defineProps(['books']);
const emit = defineEmits(['update']);

function remove(key) {
  const filtered = props.books.filter(b => b.key !== key);
  emit('update', filtered);
}
</script>

<style scoped>
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