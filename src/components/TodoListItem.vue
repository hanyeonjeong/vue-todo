<template>
  <li>
    <span class="item" :class="todoItemClass" @click="toggleItem">{{
      todoItem.title
    }}</span>
    <button @click="removeItem" class="clearBtn">삭제</button>
  </li>
</template>

<script lang="ts">
import { Todo } from "@/App.vue";
import Vue, { PropType } from "vue";

export default Vue.extend({
  props: {
    todoItem: Object as PropType<Todo>,
    index: Number,
  },

  computed: {
    todoItemClass(): string | null {
      return this.todoItem.done ? "complete" : null;
    },
  },

  methods: {
    toggleItem() {
      this.$emit("toggle", this.todoItem, this.index);
    },
    removeItem() {
      this.$emit("remove", this.index);
    },
  },
});
</script>

<style scoped>
.item {
  cursor: pointer;
}
li {
  font-size: 1.5rem;
  color: #333;
  font-family: "KOTRAHOPE";
}
.complete {
  text-decoration: line-through wavy #fd5d5d;
}

.clearBtn {
  margin-left: 0.5rem;
  background: transparent;
  border: dashed 3px #ff8080;
  border-radius: 10px;
  font-family: "KOTRAHOPE";
  font-size: 1rem;
  color: #333;
}

.clearBtn:hover {
  background: yellow;
}

.todobtn:active {
  background: yellowgreen;
  box-shadow: 4px 3px 7px 2px #00000040;
}
</style>
