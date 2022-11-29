<template>
  <section>
    <ul>
      <li
        v-for="todoItem in todoItems"
        v-bind:key="todoItem['id']"
        class="shadow"
      >
        <i class="checkBtn fas fa-check" aria-hidden="true"></i>
        {{ todoItem["todo"] }}
        <span class="removeBtn" type="button" @click="removeTodo(todoItem)">
          <i class="far fa-trash-alt" aria-hidden="true"></i>
        </span>
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  data() {
    return {
      todoItems: []
    };
  },
  methods: {
    removeTodo(todoItem) {
      var xmlHttpRequest = new XMLHttpRequest();

      xmlHttpRequest.open("DELETE", "http://localhost:8090/todo/delete");
      xmlHttpRequest.setRequestHeader("Content-Type", "application/json");
      xmlHttpRequest.onload = event => {
        console.log(event.target.response);
      };
      xmlHttpRequest.send(JSON.stringify(todoItem));

      this.todoItems.splice(
        this.todoItems.findIndex(function(element) {
          return element.id === todoItem.id;
        }),
        1
      );
    }
  },
  created() {
    var xmlHttpRequest = new XMLHttpRequest();

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
};
</script>

<style>
ul {
  list-style-type: none;
  padding-left: 0px;
  margin-top: 0;
  text-align: left;
}
li {
  display: flex;
  min-height: 50px;
  height: 50px;
  line-height: 50px;
  margin: 0.5rem 0;
  padding: 0 0.9rem;
  background: white;
  border-radius: 5px;
}

.checkBtn {
  line-height: 45px;
  color: #62acde;
  margin-right: 5px;
}
.removeBtn {
  margin-left: auto;
  color: #de4343;
}
</style>
