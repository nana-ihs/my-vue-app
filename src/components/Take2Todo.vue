<template>
  <div class="todolist">
    <h1 class="todolist_title">Todoリスト</h1>
    <div class="add-form">
      <div class="add-form_input">
        コメント
        <input
          class="add-form_input_box"
          type="text"
          placeholder="タスクを入力"
          v-model="inputTodo"
        />
        <button class="add-form_input_button" v-on:click="addTodo">追加</button>
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
      <div class="count">({{ computedTodos.length }} 件を表示）</div>
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
    computedTodos: function () {
      return this.todos.filter(function (el) {
        return this.current < 0 ? true : this.current === el.state
      }, this)
    },
  },
}
</script>
<style scoped>
.todolist {
  margin-left: 80px;
}
.todolist_title {
  margin-top: 50px;
  margin-left: 80px;
  font-size: 50px;
  width: 1180px;
  border-bottom: solid 5px #4169e1;
}
.add-form {
  margin-left: 110px;
}
.add-form_input {
  font-size: 30px;
}
.add-form_input_box {
  height: 35px;
  width: 220px;
  font-size: 20px;
}
.add-form_input_button {
  height: 40px;
  width: 62px;
  font-size: 15px;
  position: absolute;
  top: 168px;
  left: 570px;
  background-color: #6495ed;
  color: white;
  border: none;
  border-radius: 4px;
}
.status {
  padding-top: 20px;
  padding-left: 15px;
  font-size: 20px;
}
.count {
  position: absolute;
  top: 225px;
  left: 500px;
  font-size: 20px;
}
.contents {
  padding-top: 30px;
  padding-left: 130px;
  font-size: 30px;
}
.todo__text--done {
  color: #a9a9a9;
  text-decoration-line: line-through;
}
.status {
  display: inline-block;
  justify-content: space-between;
}
.todo {
  display: flex;
  border-left: solid 3px #4169e1;
}
.todo__text {
  margin-left: 10px;
}
.todo__delete {
  margin-left: 10px;
  height: 40px;
  width: 65px;
  font-size: 15px;
  border: none;
  background-color: #6495ed;
  color: white;
  border-radius: 4px;
}
</style>
