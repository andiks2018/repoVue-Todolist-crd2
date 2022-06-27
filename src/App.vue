<template>
  <div class="container">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">TODO APP SIMPLE</h5>
        <div class="row mb-5">
          <div class="col-10">
            <input @keyup.enter="add" type="text" class="form-control" v-model="todo" />
          </div>
          <div class="col-2">
            <button class="btn btn-success" @click="add">Add</button>
          </div>
        </div>
        <div class="row">
          <div class="col">
            <list 
            :todos="todos" 
            @deleteTodo="deleteTodo" 
            @doneTodo="doneTodo" 
            />
            <small>total TODO : {{ totalTODO }}</small>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import List from "./components/List.vue";
export default {
  data() {
    return {
      todo: "",
      todos: [],
    };
  },
  mounted() {
    this.todos = JSON.parse(localStorage.getItem("todos")) || [];
  },
  computed: {
    totalTODO() {
      return this.todos.length;
    },
  },
  components: { List },
  
  methods: {
    add() {
      this.todos.unshift({
        activity: this.todo,
        isDone: false,
      });
      this.todo = "";
      this.saveToLocalStorage();
    },

    deleteTodo(todoIndex) {
      this.todos = this.todos.filter((item, index) => {
        if (index != todoIndex) {
          return item;
        }
      });
      this.saveToLocalStorage();
    },
    doneTodo(todoIndex) {
      this.todos = this.todos.filter((item, index) => {
        if (index == todoIndex) {
          item.isDone = true;
        }
        return item;
      });
      this.saveToLocalStorage();
    },
    saveToLocalStorage() {
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },
  },
};
</script>
