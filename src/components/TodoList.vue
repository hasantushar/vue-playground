<template>
  <Pagination :count="todos.length / 10" @current-page="setCurrentPage" />

  <input v-model="searchText" placeholder="Search" />

  <button @click="searchTodos">Search</button>

  <ul v-if="!searchText">

    <li v-for="todo in todoPaginated" :key="todo.id">
      {{ todo.title }}
    </li>

  </ul>

  <div v-else>

    <h2>Search Result</h2>
    <h4 v-for="todo in searchResult" :key="todo.id">{{ todo.title }}</h4>

  </div>
</template>

<script setup>
import { computed } from "@vue/reactivity";
import { ref, onMounted, watch } from "vue";
import Pagination from "./Pagination.vue";

const todos = ref([]);
const searchText = ref("");
const searchResult = ref("");
const currentPage = ref("");

const timeoutRef = ref(null);

const todoPaginated = computed(() => {

  if (currentPage.value) {
    return todos.value.slice(
      (currentPage.value - 1) * 10,
      currentPage.value * 10
    );
  }

  return todos.value.slice(0, 10);
});

const searchTodos = () => {

    if(timeoutRef.value !== null) {clearTimeout(timeoutRef.value)}

    timeoutRef.value = setTimeout(()=>{

        searchResult.value = todos.value.filter((t) =>
            t.title.includes(searchText.value)
    ); 
    }, 500);

    // searchResult.value = todos.value.filter((t) =>
    //     t.title.includes(searchText.value)
    // );
};

const setCurrentPage = (index) => {
  console.log("SetCurrentPage", index);
  currentPage.value = index;
};

onMounted(async () => {
  let res = await fetch("https://jsonplaceholder.typicode.com/todos");
  todos.value = await res.json();
  // todos.value = todos.value.slice(0, 10);
  // fetch("https://jsonplaceholder.typicode.com/todos").then(async (r) => await r.json()).then(r=>console.log(r))
});

watch(searchText, (newText, oldText) => {
  if (!newText) {
    searchResult.value = null;
  } else {
    searchTodos();
  }
});
</script>
