<template>
  <div class="main">
    <header>
      <h1 class="todoTitle">연정이의 오늘 할 일</h1>
    </header>
    <main>
      <TodoInput
        :item="todoText"
        @input="updateTodoText"
        @add="addTodoItem"
      ></TodoInput>
      <div>
        <ul>
          <TodoListItem
            v-for="(todoItem, index) in todoItems"
            :key="index"
            :index="index"
            :todoItem="todoItem"
            @toggle="toggleTodoItemComplete"
            @remove="removeTodoItem"
          ></TodoListItem>
          <!-- <li>아이템 1</li>
          <li>아이템 2</li>
          <li>아이템 3</li> -->
        </ul>
      </div>
    </main>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import TodoInput from "./components/TodoInput.vue";
import TodoListItem from "./components/TodoListItem.vue";

const STORAGE_KEY = "vue-todo-ts-v1";
const storage = {
  save(todoItems: Todo[]) {
    const parsed = JSON.stringify(todoItems);
    localStorage.setItem(STORAGE_KEY, parsed);
  },
  fetch(): Todo[] {
    const todoItems = localStorage.getItem(STORAGE_KEY) || "[]";
    const result = JSON.parse(todoItems);
    return result;
  },
};

export interface Todo {
  title: string;
  done: boolean;
}

export default Vue.extend({
  components: { TodoInput, TodoListItem },
  data() {
    return {
      todoText: "",
      todoItems: [] as Todo[],
    };
  },
  methods: {
    updateTodoText(value: string) {
      this.todoText = value;
    },
    addTodoItem() {
      const value = this.todoText;
      const todo: Todo = {
        title: value,
        done: false,
      };
      this.todoItems.push(todo);
      storage.save(this.todoItems);
      // localStorage.setItem(value, value);
      this.initTodoText();
    },
    initTodoText() {
      this.todoText = "";
    },
    fetchTodoItems() {
      // this.todoItems = 1;
      this.todoItems = storage.fetch().sort((a, b) => {
        if (a.title < b.title) {
          return -1;
        }
        if (a.title > b.title) {
          return 1;
        }
        return 0;
      });
      return "hi";
    },
    removeTodoItem(index: number) {
      console.log("remove", index);
      this.todoItems.splice(index, 1);
      storage.save(this.todoItems);
    },
    toggleTodoItemComplete(todoItem: Todo, index: number) {
      this.todoItems.splice(index, 1, {
        ...todoItem,
        done: !todoItem.done,
      });
      storage.save(this.todoItems);
    },
  },
  created() {
    this.fetchTodoItems();
  },
});
</script>

<style scoped>
@font-face {
  font-family: "KOTRAHOPE";
  src: url("https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_2110@1.0/KOTRAHOPE.woff2")
    format("woff2");
  font-weight: normal;
  font-style: normal;
}

.main {
  width: 50%;
  height: auto;
  display: flex;
  flex-flow: column;
  justify-content: center;
  align-items: center;
  background: #fff7bc;
  padding: 2px;
}

.todoTitle {
  font-family: "KOTRAHOPE";
  font-size: 3rem;
}
</style>
