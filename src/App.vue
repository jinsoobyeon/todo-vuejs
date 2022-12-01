<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput v-on:addTodo="addTodo"></TodoInput>
    <TodoList v-bind:propsdata="todoItems" @removeTodo="removeTodo"></TodoList>
    <TodoFooter v-on:removeAll="clearAll"></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from "./components/TodoHeader.vue";
import TodoInput from "./components/TodoInput.vue";
import TodoList from "./components/TodoList.vue";
import TodoFooter from "./components/TodoFooter.vue";

export default {
  data() {
    return {
      todoItems: []
    };
  },
  created() {
    this.readTodos();
  },
  methods: {
    addTodo(todoItem) {
      var xmlHttpRequest = new XMLHttpRequest();

      xmlHttpRequest.open("POST", "http://localhost:8090/todo/create");
      xmlHttpRequest.setRequestHeader("Content-Type", "application/json");
      xmlHttpRequest.onload = event => {
        if (JSON.parse(event.target.response)) {
          this.readTodos();
        }
      };
      xmlHttpRequest.send(JSON.stringify({ todo: todoItem }));
    },
    clearAll() {
      var xmlHttpRequest = new XMLHttpRequest();

      xmlHttpRequest.open("DELETE", "http://localhost:8090/todo/deleteAll");
      xmlHttpRequest.onload = event => {
        if (JSON.parse(event.target.response)) {
          this.todoItems = [];
        }
      };
      xmlHttpRequest.send();
    },
    removeTodo(todoItem) {
      var xmlHttpRequest = new XMLHttpRequest();

      xmlHttpRequest.open("DELETE", "http://localhost:8090/todo/delete");
      xmlHttpRequest.setRequestHeader("Content-Type", "application/json");
      xmlHttpRequest.onload = event => {
        if (JSON.parse(event.target.response)) {
          this.todoItems.splice(
            this.todoItems.findIndex(function(element) {
              return element.id === todoItem.id;
            }),
            1
          );
        }
      };
      xmlHttpRequest.send(JSON.stringify(todoItem));
    },
    readTodos() {
      var xmlHttpRequest = new XMLHttpRequest();

      this.todoItems = [];

      xmlHttpRequest.open("GET", "http://localhost:8090/todo/");
      xmlHttpRequest.onload = event => {
        if (JSON.parse(event.target.response).length > 0) {
          for (var i = 0; i < JSON.parse(event.target.response).length; i++) {
            this.todoItems.push(JSON.parse(event.target.response)[i]);
          }
        }
      };
      xmlHttpRequest.send();
    }
  },
  components: {
    TodoHeader: TodoHeader,
    TodoInput: TodoInput,
    TodoList: TodoList,
    TodoFooter: TodoFooter
  }
};
</script>

<style>
body {
  text-align: center;
  background-color: #f6f6f8;
}
input {
  border-style: groove;
  width: 200px;
}
button {
  border-style: groove;
}

.shadow {
  box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.3);
}
</style>
