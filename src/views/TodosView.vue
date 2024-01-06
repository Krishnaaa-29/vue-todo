<script setup>
import TodoCreator from "@/components/TodoCreator.vue";
import TodoItem from "@/components/TodoItem.vue";
import { uid } from "uid";
import { ref, computed } from "vue";
import { Icon } from "@iconify/vue";

const todoList = ref([]);

const todoCompleted = computed(() => {
  return todoList.value.every((item) => item.isCompleted === true);
});

const setTodoListLocalStorage = () => {
  localStorage.setItem("todoList", JSON.stringify(todoList.value));
};

const getTodoListLocalStorage = () => {
  const savedTodoList = JSON.parse(localStorage.getItem("todoList"));
  if (savedTodoList) {
    todoList.value = savedTodoList;
  }
};

getTodoListLocalStorage();

const createTodoList = (todo) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: false,
    isEditing: null,
  });
  setTodoListLocalStorage();
};

const toggleTodoComplete = (pos) => {
  todoList.value[pos].isCompleted = !todoList.value[pos].isCompleted;
  setTodoListLocalStorage();
};

const editTodo = (pos) => {
  todoList.value[pos].isEditing = !todoList.value[pos].isEditing;
  setTodoListLocalStorage();
};

const updateTodo = (todoVal, pos) => {
  todoList.value[pos].todo = todoVal;
  setTodoListLocalStorage();
};

const deleteTodo = (id) => {
  todoList.value = todoList.value.filter((item) => item.id !== id);
  setTodoListLocalStorage();
};
</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="createTodoList" />
    <ul v-if="todoList.length > 0" class="todo-list">
      <TodoItem
        v-for="(todo, index) in todoList"
        :todo="todo"
        :index="index"
        @toggle-complete="toggleTodoComplete"
        @edit-todo="editTodo"
        @update-todo="updateTodo"
        @delete-todo="deleteTodo"
      />
    </ul>
    <p v-else class="todos-msg">
      <Icon icon="noto-v1:sad-but-relieved-face" width="22" />
      <span>You have no TODO's to complete. Go have fun!</span>
    </p>
    <p v-if="todoCompleted && todoList.length > 0" class="todos-msg">
      <Icon icon="noto-v1:party-popper" />
      <span>You have completed all the tasks!</span>
    </p>
  </main>
</template>

<style lang="scss" scoped>
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }

  .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .todos-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>
