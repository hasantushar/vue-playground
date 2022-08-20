<template>

  <div class="pagination">

    <div v-for="index in count" :key="index">

      <button @click="currentPage(index)">{{ index }}</button>

    </div>
    
  </div>
</template>

<script setup>
import { ref, toRef, defineProps } from "vue";

defineProps({ count: Number });

const emit = defineEmits(["current-page"]);

const currentPage = (index) => {
  console.log("page", index);
  emit("current-page", index);
};

</script>

<style scoped>
.pagination {
  display: flex;
}
</style>
