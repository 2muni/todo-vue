<template>
 <div>
   <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo">
    <todo-item v-for="todo in todos" :key="todo.id" :todo="todo" @removeTodo="removeTodo" />
  </div>
</template>

<script>
import TodoItem from './TodoItem'

export default {
  name: 'TodoList',
  components: {
    TodoItem,
  },
  data () {
    return {
      newTodo: '',
      idForTodo: 2,
      todos: [
        {
          'id': 0,
          'title': 'Finish Vue Study',
          'completed': false,
          'editing': false,
        },
        {
          'id': 1,
          'title': 'Take over world',
          'completed': false,
          'editing': false,
        },
      ]
    }
  },
  methods: {
    addTodo() {
      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false,
        editing: false,
      })
      this.newTodo = ''
      this.idForTodo++
    },
    removeTodo(id) {
      this.todos.splice(id, 1)
    }
  },
}
</script>

<style lang="scss" scoped>
.todo-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;
  &:focus {
    outline: 0;
  }
}
</style>
