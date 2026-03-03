<script setup lang="js">
import todos from '@/assets/todos.js'
import TodoList from '@/components/todo/TodoList.vue'
</script>

<template>
  <h2>List</h2>

  <TodoList :todos="todos" />
</template>
