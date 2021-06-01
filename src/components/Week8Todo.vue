<template>
  <div class="todolist">
    <h1 class="todolist_title">Todoリスト</h1>
    <div class="add-form">
      <div class="add-form_input">
        コメント
        <input type="text" placeholder="タスクを入力" v-model="inputTodo" />
        <button v-on:click="addTodo">追加</button>
      </div>
      <div class="status" v-for="item in items" v-bind:key="item.id">
        <input
          type="radio"
          name="show"
          :id="item.id"
          :value="item.value"
          v-model="show"
        />
        <label :for="item.id">{{ item.text }}</label>
      </div>
      <div class="contents">
        <div v-if="showTasks.length <= 0">タスクなし！</div>
        <div v-for="(todo, index) in showTasks" v-bind:key="index" class="todo">
          <div class="todo__checkbox">
            <input type="checkbox" v-model="todo.isDone" />
          </div>
          <div v-if="todo.isDone" class="todo__text todo__text--done">
            {{ todo.text }}
          </div>
          <div v-else class="todo__text">{{ todo.text }}</div>
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
      items: [
        { id: "all", value: 0, text: "すべて" },
        { id: "doing", value: 1, text: "作業中" },
        { id: "done", value: 2, text: "完了" },
      ],
      show: 0,
    }
  },
  methods: {
    addTodo() {
      if (this.inputTodo !== "") {
        this.todos.push({ text: this.inputTodo, isDone: false })
      }
      this.inputTodo = ""
    },
    deleteTodo(index) {
      this.todos.splice(index, 1)
    },
  },
  computed: {
    showTasks() {
      switch (this.show) {
        case 0:
          return this.todos
        case 1:
          return this.todos.filter((e) => !e.isDone)
        case 2:
          return this.todos.filter((e) => e.isDone)
        default:
          return []
      }
    },
  },
}
</script>
<style scoped>
.todolist {
  margin-left: 50px;
}
.todo__text--done {
  color: #a9a9a9;
}
.todolist_title {
  margin-top: 50px;
}

.status {
  display: inline-block;
  justify-content: space-between;
}
.todo {
  display: flex;
}
.todo__text {
  margin-left: 10px;
}
.todo__delete {
  margin-left: 10px;
}
</style>
