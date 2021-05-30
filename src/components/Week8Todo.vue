<template>
  <div class="todolist">
    <h1>Todoリスト</h1>
    <div class="add-form">
      コメント
      <input plaveholder="タスクを入力" v-model="inputTodo" />
      <button v-on:click="addTodo">追加</button>
      <div class="contents">
        <div v-for="(todo, index) in todos" v-bind:key="index" class="todo">
          {{ index }}
          {{ todo }}
          <button v-on:click="deleteTodo(index)" class="todo__delete">
            削除
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      inputTodo: "",
      todos: [],
    }
  },
  methods: {
    addTodo() {
      this.todos.push(this.inputTodo), (this.inputTodo = "")
    },
    deleteTodo(index) {
      this.todos.splice(index, 1)
    },
  },
}
</script>
