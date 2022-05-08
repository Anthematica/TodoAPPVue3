<script setup>
import { toRaw } from "vue";
import ky from "ky";

const emit = defineEmits(["delete"]);

const props = defineProps({
  todos: {
    required: true,
    type: Array,
  },
});

// console.log("Valores que vienen desde el papa: ", values.todos);

function handleDelete(id) {
  emit("delete", id);
}

async function handleChange(todo) {
  const resp = ky
    .patch(`http://127.0.0.1:8000/api/v1/todos/${todo.id}`, {
      json: {
        status: !todo.status,
      },
    })
    .json();
}
</script>
<template>
  <div class="todos">
    <p>
      <input
        type="checkbox"
        :checked="todos.status"
        name="status"
        @change="handleChange(todos)"
      />
      {{ todos.name }}
    </p>
    <p><span>By: </span> {{ todos.user_id.name }}</p>

    <button @click="handleDelete(todos.id)" className="delete_todo">X</button>
  </div>
</template>
<style>
.todos {
  width: 500px;
  background-color: #4784de;
  border-radius: 10px;
  padding: 15px;
  position: relative;
}

.todos p {
  color: white;
}

.delete_todo {
  position: absolute;
  inset-inline-end: 6px;
  inset-block-start: 6px;
  cursor: pointer;
  background-color: white;
  border: none;
  font-weight: bolder;
  border-radius: 2px;
}

.completed:checked {
  background-color: red;
}
</style>
