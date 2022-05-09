<script setup>
import ky from "ky";
import { inject, ref } from "vue";

const { handleDelete } = inject("todos");

const props = defineProps({
  todos: {
    required: true,
    type: Array,
  },
});

console.log("A verrr", props.todos);

const isEditing = ref(false);

const name = ref(props.todos.name);

// function handleUpdateText() {}

async function handleChange(todo, text) {
  const resp = await ky
    .patch(`http://127.0.0.1:8000/api/v1/todos/${todo.id}`, {
      json: {
        status: !todo.status,
        name: text,
      },
    })
    .json();

  console.log("Resp: ", resp.data.status);
  console.log("Props: ", props.todos.status);

  if (props.todos.id === todo.id) {
    props.todos.name = text;
  }

  isEditing.value = false;

  props.todos.status = resp.data.status;
}
</script>

<template>
  <div class="todos">
    <template v-if="isEditing">
      <input type="text" v-model="name" name="name" />
      <button @click="handleChange(todos, name)">Save</button>
      <button @click="isEditing = false">Cancel</button>
    </template>
    <template v-else>
      <p :class="todos.status ? 'todo_done' : 'todo_info'">
        <input
          type="checkbox"
          :checked="todos.status"
          name="status"
          @change="handleChange(todos, name)"
        />
        {{ todos.name }}
        <button class="edit_todo" @click="isEditing = true">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
            <!--! Font Awesome Pro 6.1.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. -->
            <path
              d="M490.3 40.4C512.2 62.27 512.2 97.73 490.3 119.6L460.3 149.7L362.3 51.72L392.4 21.66C414.3-.2135 449.7-.2135 471.6 21.66L490.3 40.4zM172.4 241.7L339.7 74.34L437.7 172.3L270.3 339.6C264.2 345.8 256.7 350.4 248.4 353.2L159.6 382.8C150.1 385.6 141.5 383.4 135 376.1C128.6 370.5 126.4 361 129.2 352.4L158.8 263.6C161.6 255.3 166.2 247.8 172.4 241.7V241.7zM192 63.1C209.7 63.1 224 78.33 224 95.1C224 113.7 209.7 127.1 192 127.1H96C78.33 127.1 64 142.3 64 159.1V416C64 433.7 78.33 448 96 448H352C369.7 448 384 433.7 384 416V319.1C384 302.3 398.3 287.1 416 287.1C433.7 287.1 448 302.3 448 319.1V416C448 469 405 512 352 512H96C42.98 512 0 469 0 416V159.1C0 106.1 42.98 63.1 96 63.1H192z"
            />
          </svg>
        </button>
      </p>
      <p class="author_name"><span>By: </span> {{ todos.user_id.name }}</p>
      <button @click="handleDelete(todos.id)" className="delete_todo">X</button>
    </template>
  </div>
</template>

<style>
.todos {
  width: 500px;
  background-color: #5784c9;
  border-radius: 10px;
  padding: 10px;
  position: relative;
  box-shadow: 10px 10px 5px -6px rgba(0, 0, 0, 0.75);
  -webkit-box-shadow: 10px 10px 5px -6px rgba(0, 0, 0, 0.75);
  -moz-box-shadow: 10px 10px 5px -6px rgba(0, 0, 0, 0.75);
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

.todo_info {
  display: flex;
  align-items: center;
  column-gap: 10px;
}

.todo_done {
  text-decoration-line: line-through;
}

.edit_todo {
  border: none;
  border-radius: 4px;
  cursor: pointer;
  padding: 2px 4px;
}

.edit_todo svg {
  width: 12px;
  height: 12px;
}
</style>
