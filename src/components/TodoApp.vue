<script setup lang="ts">
import { Ref, ref } from 'vue';
interface ITodo {
  id: string;
  description: string;
  isComplete: boolean;
}
const todoList: Ref<ITodo[]> = ref([]);

const todo = ref('');

const addTodo = (_) => {
  if (todo.value !== '') {
    todoList.value.push({ id: crypto.randomUUID(), description: todo.value, isComplete: false });
    todo.value = '';
  } else {
    alert('Todo should not be empty!')
  }
}
const removeTodo = (id: string) => {
  todoList.value
    = todoList.value.filter(value => value.id != id);
}
const markTodoComplete = (id: string) => {
  todoList.value = todoList.value.map(value => value.id === id ? { ...value, isComplete: true } : value)
}
</script>

<template>
  <main class="p-14 flex justify-center">
    <div class="w-[500px]">
      <div class="flex justify-center gap-4 mb-8">
        <input v-model="todo" class="input input-primary" />
        <button @click="addTodo" class="btn btn-primary"> Add Todo</button>
      </div>
      <p class="text-lg font-medium mb-2">Pending</p>
      <ul v-for="todo in todoList" :key="todo.id">
        <div v-if="!todo.isComplete" class="flex flex-col my-2 p-4 rounded-md dark:bg-amber-600 bg-amber-300 ">
          <p>{{ todo.description }}</p>
          <div class="flex justify-end gap-2">
            <button @click="markTodoComplete(todo.id)" class="btn btn-success btn-sm">Mark as Complete</button>
            <button @click="removeTodo(todo.id)" class="btn btn-error btn-sm">Remove</button>
          </div>
        </div>
      </ul>
      <p class="text-lg font-medium my-2">Completed</p>

      <ul v-for="todo in todoList" :key="todo.id">
        <div v-if="todo.isComplete"
          class="flex justify-between items-center p-3 my-2 rounded-md dark:bg-green-700 bg-green-500  ">
          <p>{{ todo.description }}</p>
          <button @click="removeTodo(todo.id)" class="btn btn-error btn-sm">Remove</button>
        </div>
      </ul>
    </div>
  </main>
</template>

<style scoped></style>
