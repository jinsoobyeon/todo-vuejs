<template>
  <div class="clearAllContainer">
    <span class="clearAllBtn" @click="clearTodo"> Clear All</span>
  </div>
</template>

<script>
export default {
  methods: {
    clearTodo() {
      var xmlHttpRequest = new XMLHttpRequest();

      xmlHttpRequest.open("DELETE", "http://localhost:8090/todo/deleteAll");
      xmlHttpRequest.onload = event => {
        console.log(event.target.response);
      };
      xmlHttpRequest.send();
    }
  }
};
</script>

<style scoped>
.clearAllContainer {
  width: 8.5rem;
  height: 50px;
  line-height: 50px;
  background-color: white;
  border-radius: 5px;
  margin: 0 auto;
}
.clearAllBtn {
  color: #e20303;
  display: block;
}
</style>
