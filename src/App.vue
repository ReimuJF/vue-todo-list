<script setup>
import { ref, onMounted, provide } from 'vue';
import inputForm from './components/inputFormComponent.vue'
import todoList from './components/todoListComponent.vue'


const data = ref([]);
const errorMessage = ref(null);

onMounted(async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos/');
    if (!response.ok) {
      throw new Error("Something went wrong");
    }
    const json = await response.json();
    data.value = Object.values(json);
  } catch (error) {
    errorMessage.value = error.message;
  }
});

provide('todoList', data);
provide("error", errorMessage);

</script>

<template>
  <h1>Vue TODO List</h1>
  <inputForm />
  <todoList />
</template>

<style scoped></style>
