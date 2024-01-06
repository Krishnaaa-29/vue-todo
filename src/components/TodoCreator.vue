<script setup>
import { reactive, ref } from "vue";
import TodoButton from "./TodoButton.vue";
const emit = defineEmits(["create-todo"]);
// Ways to access data which needs to be reactive in nature, either by ref or reactive
// const todo = ref(""); // Method 1
const todoState = reactive({
  // Method 2
  todo: "",
  invalid: null,
  errMsg: "",
});

const createTodo = () => {
  todoState.invalid = null;
  if (todoState.todo !== "") {
    emit("create-todo", todoState.todo);
    todoState.todo = "";
    return;
  }

  todoState.invalid = true;
  todoState.errMsg = "Todo Value cannot be empty";
};
</script>

<template>
  <div class="input-wrap" :class="{ 'input-err': todoState.invalid }">
    <input type="text" v-model="todoState.todo" />
    <TodoButton @click="createTodo">Create</TodoButton>
  </div>
  <p v-if="todoState.invalid" class="err-msg">{{ todoState.errMsg }}</p>
  <!-- <p v-show="todoState.invalid" class="err-msg">{{ todoState.errMsg }}</p> -->
  <!-- diff between v-show and v-if is that v-if removes the html while v-show sets display to none when false -->
</template>

<style lang="scss" scoped>
.input-wrap {
  display: flex;
  transition: 250ms ease;
  border: 2px solid #41b080;

  &.input-err {
    border-color: red;
  }

  &:focus-within {
    box-shadow: 0 -4px 6px -1px rgb(0 0 0 / 0.1),
      0 -2px 4px -2px rgb(0 0 0 / 0.1);
  }

  input {
    width: 100%;
    padding: 8px 6px;
    border: none;

    &:focus {
      outline: none;
    }
  }
}
.err-msg {
  margin-top: 6px;
  font-size: 12px;
  text-align: center;
  color: red;
}
</style>
