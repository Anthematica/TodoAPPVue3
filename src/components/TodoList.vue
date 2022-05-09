<script setup>
import TodoItem from "./TodoItem.vue";
defineProps({
  todos: {
    required: true,
    type: Array,
  },
});
</script>
<template>
  <div v-for="todo in todos" :key="todo.id" className="container_todo">
    <TodoItem :todos="todo" />
  </div>
</template>
<style>
.container_todo {
  margin-block-start: 16px;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  row-gap: 16px;
}
</style>
