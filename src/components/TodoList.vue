<template>
{{currentPage}}
<input v-model="searchText" placeholder="Search" />

<button @click="searchTodos">Search</button>
<ul>
    <li v-for="todo in todos" :key="todo.id">
    {{ todo.title }}
    </li>
</ul>

<h3>{{ searchResult }}</h3>

<Pagination :count="todos.length" />

</template>

<script setup>
import { ref, onMounted, toRef } from "vue";
import Pagination from "./Pagination.vue";

const todos = ref([]);
const searchText = ref("");
const searchResult = ref("")

const searchTodos = () => {
    searchResult.value = todos.value.filter(t => t.title === searchText.value)[0]
}

onMounted(async () => {
    let res = await fetch("https://jsonplaceholder.typicode.com/todos")
    todos.value = await res.json();
    todos.value = todos.value.slice(0, 10);
})


</script>
