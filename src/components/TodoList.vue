<template>
  <section>
    <ul>
      <li v-for="todoItem in todoItems" v-bind:key="todoItem['id']">
        {{ todoItem["todo"] }}
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

<style></style>
