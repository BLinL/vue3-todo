<script setup>
import TodoList from './components/TodoList.vue';
</script>

<template>
  <div class="list-container">
    <TodoList />
    <TodoList />
    <TodoList />
    <TodoList />
    <TodoList />
    <TodoList />
    <TodoList />
    <TodoList />
    <TodoList />
  </div>
</template>

<style scoped>
.list-container {
  display: flex;
  width: 100%;
  align-items: flex-start;
  flex-wrap: wrap;
  justify-content: flex-start;
}
</style>
