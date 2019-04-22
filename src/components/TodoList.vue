<template>
  <div>
    <p class="tasks">Completed Tasks: {{todos.filter(todo => {return todo.done === true}).length}}</p>
    <p class="tasks">Pending Tasks: {{todos.filter(todo => {return todo.done === false}).length}}</p>
    <br />
    <div class = 'ui three column centered grid' id="todo-filter">
      <input type="checkbox" id="completed" value="completed" v-model="filterStatus">
      <label for="completed">Completed</label>
      <input type="checkbox" id="pending" value="pending" v-model="filterStatus">
      <label for="pending">Pending</label>
    </div>
    <todo v-on:delete-todo="deleteTodo" v-on:complete-todo="completeTodo" v-on:uncomplete-todo="uncompleteTodo" v-for="todo in filterTodos" :todo.sync="todo"></todo>
  </div>
</template>

<script type = "text/javascript" >
import Todo from './Todo';

export default {
  props: ['todos', 'checkedStatus'],
  components: {
    Todo,
  },
  data: function () {
    return {
      filterStatus: this.checkedStatus
    }
  },
  methods: {
    deleteTodo(todo) {
        const todoIndex = this.todos.indexOf(todo);
        this.todos.splice(todoIndex, 1);
    },
    completeTodo(todo) {
      const todoIndex = this.todos.indexOf(todo);
      this.todos[todoIndex].done = true;
    },
    uncompleteTodo(todo) {
      const todoIndex = this.todos.indexOf(todo);
      this.todos[todoIndex].done = false;
    },
  },
  computed: {
    filterTodos: function () {
      return this.todos.filter(todo => {
        if (this.filterStatus.includes('pending') && this.filterStatus.includes('completed'))
          return todo;
        else if (this.filterStatus.includes('completed'))
          return todo.done == true;
        else if (this.filterStatus.includes('pending'))
          return todo.done == false;
        else
          return
      })
    }
  },
};
</script>

<style scoped>
p.tasks {
  text-align: center;
}
</style>