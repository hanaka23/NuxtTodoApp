<script setup lang="ts">
import { ref, computed } from "vue";

const todos = ref<Todos[]>([]);
const title = ref("");
const showAll = ref(true)
const filterdTodos = computed(()=>{
  if(showAll.value){
    return todos.value
  }
  return todos.value.filter(todo => !todo.done)
})

interface Todos {
  id: number;
  done: boolean;
  title: string;
}

function handleSubmit() {
  const newTodo: Todos = {
    id: Date.now(),
    done: false,
    title: title.value,
  };
  todos.value = [newTodo, ...todos.value];
  title.value = "";
}
function toggleDone(todo: Todos) {
  todo.done = !todo.done;
}
function deleteTodo(id:number) {
  todos.value = todos.value.filter((todo) => todo.id !== id)
}


</script>

<template>
  <div class="w-[600px] m-auto bg-gray-100 mt-[300px] rounded-[20px] p-[24px] ">
    <h1 class="text-3xl font-bold text-center mb-[12px]">Todoリスト</h1>
    <form @submit.prevent="handleSubmit" class="text-center">
      <input v-model="title" type="text" class="mr-[12px] rounded-[4px]" required />
      <button type="submit" class="px-[8px] py-[4px] rounded-[4px] text-sm bg-[#55e0c7]">追加</button>
    </form>

    <div class="m-auto w-fit flex gap-[24px] pt-[24px] mb-[12px]">
      <button @click="showAll=true" class="text-blue-600">全て</button>
      <button @click="showAll=false" class="text-blue-600">未完了のみ</button>
    </div>

    <table class="bg-white w-[500px] m-auto ">
      <tr v-for="todo in filterdTodos" :key="todo.id" class="px-[24px] py-[8px] flex justify-between border-b-[1px] border-gray-300">
        <div class="flex gap-[12px]">
          <button @click="toggleDone(todo)">
            {{ todo.done ? "✅" : "◻️" }}
          </button>
          <td>{{ todo.title }}</td>
        </div>
        <button @click="deleteTodo(todo.id)" class="px-[8px] py-[4px] rounded-[4px] text-sm bg-[#ef87c2]">削除</button>
      </tr>
    </table>
  </div>
</template>
