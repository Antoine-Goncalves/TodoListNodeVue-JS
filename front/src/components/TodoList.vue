<template>
  <div class="container">
    <form>
      <div class="input-group mb-3">
        <input
          type="text"
          class="form-control"
          placeholder="Ajouter quelque chose"
          aria-label="Ajouter quelque chose"
          aria-describedby="basic-addon2"
          v-model="newTodo"
        />
        <div class="input-group-append">
          <button class="btn btn-outline-secondary" type="button" @click="addTodo()">Ajouter</button>
        </div>
      </div>

      <div v-for="todo in todos" :key="todo">
        <div class="modal" tabindex="-1" role="dialog">
          <div class="modal-dialog" role="document">
            <div class="modal-content">
              <div class="modal-header">
                <h5 class="modal-title">Modification</h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                  <span aria-hidden="true">&times;</span>
                </button>
              </div>
              <div class="modal-body">
                <input type="text" v-model="changeTodo" />
              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-dismiss="modal">Annuler</button>
                <button
                  v-for="todo in todos"
                  :key="todo.id"
                  type="button"
                  class="btn btn-primary"
                  @click="updateTodo(todo.id)"
                >Modifier {{todo.id}}</button>
              </div>
            </div>
          </div>
        </div>
        <li class="card-title d-flex align-items-center justify-content-between h1">
          {{ todo.title }}
          <div>
            <button
              type="button"
              class="btn btn-outline-success btn-sm mr-2"
              data-toggle="modal"
              data-target=".modal"
            >Éditer</button>

            <button type="button" class="btn btn-outline-dark btn-sm" @click="deleteTodo(todo.id)">X</button>
          </div>
        </li>

        <ul>
          <li v-for="item in todo.todoItems" :key="item" class="text-left">{{item.content}}</li>
        </ul>
      </div>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "TodoList",
  data() {
    return {
      todos: null,
      newTodo: "",
      changeTodo: ""
    };
  },
  methods: {
    async getAllTodos() {
      const response = await axios.get("http://localhost:4000/api/todos");

      this.todos = response.data;
    },
    addTodo() {
      axios.post("http://localhost:4000/api/todos", {
        title: this.newTodo
      });
      this.newTodo = "";
    },
    deleteTodo(id) {
      axios.delete(`http://localhost:4000/api/todos/${id}`);
    },
    updateTodo(id) {
      axios.patch(`http://localhost:4000/api/todos/${id}`, {
        title: this.changeTodo
      });
    }
  },
  mounted() {
    this.getAllTodos();
  }
};
</script>

<style scoped>
</style>
