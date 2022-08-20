<template>
<Pagination :count="todos.length/10" @current-page="setCurrentPage" />

<input v-model="searchText" placeholder="Search" />

<button @click="searchTodos">Search</button>
<ul>
    <li v-for="todo in todoPaginated" :key="todo.id">
    {{ todo.title }}
    </li>
</ul>

<h3>{{ searchResult }}</h3>

</template>

<script setup>
import { computed } from "@vue/reactivity";
import { ref, onMounted, toRef } from "vue";
import Pagination from "./Pagination.vue";

const todos = ref([]);
const searchText = ref("");
const searchResult = ref("");
const currentPage = ref("");

const todoPaginated = computed(() => {
    if(currentPage.value) {
        return todos.value.slice((currentPage.value -1) * 10 , currentPage.value * 10);
    }

    return todos.value.slice(0 , 10);
})

const searchTodos = () => {
    searchResult.value = todos.value.filter(t => t.title === searchText.value)[0]
}

const setCurrentPage = (index) => {
    console.log('SetCurrentPage', index);
    currentPage.value = index;
}

onMounted(async () => {
    let res = await fetch("https://jsonplaceholder.typicode.com/todos")
    todos.value = await res.json();
    // todos.value = todos.value.slice(0, 10);
    // fetch("https://jsonplaceholder.typicode.com/todos").then(async (r) => await r.json()).then(r=>console.log(r))

})


</script>
