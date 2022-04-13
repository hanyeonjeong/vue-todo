<template>
  <div class="contents">
    <label for="todo-input" class="todoLabel">오늘 할 일 : </label>
    <input
      id="todo-input"
      class="todoinput"
      type="text"
      :value="item"
      @input="handleInput"
    />
    <button @click="addTodo" type="button" class="todobtn">추가</button>
  </div>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  // props: ["item"],
  props: {
    item: {
      type: String,
      required: true,
    },
  },
  methods: {
    handleInput(event: InputEvent) {
      // console.log(event);
      // if (!event.target) {
      //   return;
      // }
      const eventTarget = event.target as HTMLInputElement;
      this.$emit("input", eventTarget.value);
    },
    addTodo() {
      this.$emit("add");
    },
  },
});
</script>

<style scoped>
.contents {
  display: flex;
  justify-content: start;
  align-items: center;
}
.todoLabel {
  display: block;
  margin-bottom: 1rem;
  font-family: "KOTRAHOPE";
  font-size: 2rem;
}
.todoinput {
  box-sizing: border-box;
  background: transparent;
  padding: 0.7rem;
  border: solid 3px transparent;
  border-bottom: dashed 3px #ff8080;
  color: #333;
  margin-bottom: 20px;
  font-family: "KOTRAHOPE";
  font-size: 1.5rem;
}
.todoinput:focus {
  outline: none;
  border: solid 3px #ff8080;
}

.todobtn {
  margin-left: 0.5rem;
  padding: 0.3rem 1rem;
  background: transparent;
  border: dashed 3px #ff8080;
  border-radius: 10px;
  font-family: "KOTRAHOPE";
  font-size: 1rem;
  color: #333;
}

.todobtn:hover {
  background: #c0eda6;
}

.todobtn:active {
  background: #c0eda6;
  box-shadow: 4px 3px 7px 2px #00000040;
}
</style>
