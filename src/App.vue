<template>
  <div id="app">
    <component-header
      v-bind:todos="todos"
      v-on:AddTodoEvent="addTodo"
      v-on:deleteTodoEvent="deleteTodo"
      v-on:updateTodo="updateTodo"
    />
  </div>
</template>

<script>
import axios from "axios";
import componentHeader from "./components/ComponentHeader.vue";
const baseURL = "http://localhost:3000/todos";

export default {
  components: {
    componentHeader
  },
  name: "app",
  data() {
    return {
      todos: []
    };
  },
  async created() {
    try {
      const res = await axios.get(baseURL);
      this.todos = res.data;
    } catch (error) {
      console.error("ERROR :", error);
    }
  },
  methods: {
    async addTodo(todoName) {
      const res = await axios.post(baseURL, {
        name: todoName,
        active: "all"
      });
      this.todos = [...this.todos, res.data];
    },
    async deleteTodo(data) {
      try { 
        if (data.length > 0) {
          for (let key in data) {
            await axios.delete(baseURL + "/" + `${data[key].id}`);
          }
        } else {
          await axios.delete(baseURL + "/" + `${data.id}`);
        }
        // console.log(data );
        const res = await axios.get(baseURL);
        this.todos = res.data;
      } catch (error) {
        console.error("ERROR :", error);
      }
    },
    async updateTodo(data) {
      try {
        if (data.length > 0) {
          for (let key in data) {
            await axios.put(baseURL + "/" + `${data[key].id}`, data[key]);
          }
        } else {
          await axios.put(baseURL + "/" + `${data.id}`, data);
        }
        const res = await axios.get(baseURL);
        this.todos = res.data;
      } catch (error) {
        console.error("ERROR :", error);
      }
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
