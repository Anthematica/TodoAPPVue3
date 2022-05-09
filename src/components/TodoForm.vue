<script setup>
import ky from "ky";
import { onBeforeMount, ref, inject } from "vue";

const users = ref([]);
const values = ref({
  name: "",
  user_id: "",
});

const { handleSubmit } = inject("todos");

onBeforeMount(async () => {
  const resp = await ky.get("http://127.0.0.1:8000/api/v1/users").json();
  users.value = resp.data;
});

function handleSubmitTodo() {
  handleSubmit(values.value);
}
</script>

<template>
  <form @submit.prevent="handleSubmitTodo" className="form_container">
    <h1>Write your things to do down here, little bastard</h1>
    <div className="input_container">
      <input
        name="name"
        type="text"
        v-model="values.name"
        className="chat_input"
        placeholder="Type something to do..."
      />
      <button type="submit" className="chat_input_button">
        <svg
          width="20"
          height="20"
          viewBox="0 0 20 20"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M19.4354 0.581983C18.9352 0.0685981 18.1949 -0.122663 17.5046 0.0786645L1.408 4.75952C0.679698 4.96186 0.163487 5.54269 0.0244302 6.28055C-0.117628 7.0315 0.378575 7.98479 1.02684 8.38342L6.0599 11.4768C6.57611 11.7939 7.24238 11.7144 7.66956 11.2835L13.4329 5.4843C13.723 5.18231 14.2032 5.18231 14.4934 5.4843C14.7835 5.77623 14.7835 6.24935 14.4934 6.55134L8.71999 12.3516C8.29181 12.7814 8.21178 13.4508 8.52691 13.9702L11.6022 19.0538C11.9623 19.6577 12.5826 20 13.2628 20C13.3429 20 13.4329 20 13.513 19.9899C14.2933 19.8893 14.9135 19.3558 15.1436 18.6008L19.9156 2.52479C20.1257 1.84028 19.9356 1.09537 19.4354 0.581983Z"
            fill="white"
          />
        </svg>
      </button>
    </div>
    <label className="choose_user">
      Choose a user
      <select v-model="values.user_id">
        <option v-for="user in users" :key="user.id" :value="user.id">
          {{ user.name }}
        </option>
      </select>
    </label>
  </form>
</template>

<style>
.input_container {
  width: 500px;
  height: 40px;
  background-color: white;
  border: 1px solid gray;
  border-radius: 48px;
  margin: 24px 32px;
  display: flex;
  align-items: center;
  padding-inline-start: 36px;
  padding-inline-end: 12px;
  column-gap: 34px;
}

.chat_input {
  outline: none;
  border: none;
  width: 90%;
  font-family: "Poppins", sans-serif;
  font-size: 16px;
  font-weight: 400;
  color: #121212;
}

.chat_input_button {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  background-color: #4784de;
  display: flex;
  justify-content: center;
  align-items: center;
  border: none;
  cursor: pointer;
}

.form_container {
  display: flex;
  flex-direction: column;
  width: 100%;
  justify-content: center;
  align-items: center;
}

.form_container select {
  height: 30px;
}

.choose_user {
  font-size: 20px;
}
</style>
