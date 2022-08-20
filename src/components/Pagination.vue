<template>
Hello

<div v-for="index in count" :key="index">
    <button @click="currentPage(index)" >{{ index }}</button>
</div>
</template>

<script setup>
import { ref, toRef, defineProps } from 'vue';

defineProps({count: Number})


const currentPage = (index) => {
    console.log("page", index);
}
</script>