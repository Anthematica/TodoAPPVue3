<script setup>
import ky from "ky";
import TodoForm from "./components/TodoForm.vue";
import TodoList from "./components/TodoList.vue";

import { onBeforeMount, ref, provide } from "vue";

provide("todos", {
  handleDelete,
  handleSubmit,
});

const todos = ref([]); //crea una referencia

onBeforeMount(async () => {
  const resp = await ky.get("http://127.0.0.1:8000/api/v1/todos").json();
  todos.value = resp.data;
  console.log(todos.value);
});

async function handleSubmit(values) {
  const resp = await ky
    .post("http://127.0.0.1:8000/api/v1/todos", {
      json: {
        name: values.name,
        user_id: values.user_id,
      },
    })
    .json();

  todos.value.unshift(resp.data);
}

async function handleDelete(id) {
  await ky.delete(`http://127.0.0.1:8000/api/v1/todos/${id}`);
  console.log("Holaaaaaa soy eleminar");

  const item = todos.value.findIndex((i) => i.id === id);

  todos.value.splice(item, 1);
}
</script>

<template>
  <TodoForm />
  <TodoList :todos="todos" />
</template>

<style></style>
