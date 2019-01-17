<template>
  <div>
    <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo">
    
    <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
    <todo-item v-for="todo in todosFiltered" :key="todo.id" :todo="todo" :checkAll="!anyRemaining" @removeTodo="removeTodo" @doneEdit="doneEdit"/>
    </transition-group>

    <div class="extra-container">
      <todo-check-all :anyRemaining="anyRemaining" @checkAllTodos="checkAllTodos"/>
      <todo-items-remaining :remaining="remaining"/>
    </div>

    <div class="extra-container">
      <todo-filtered :filter="filter" @setFilter="setFilter"/>
      <transition name="fade">
      <todo-clear-completed :showClearCompletedButton="showClearCompletedButton" @clearCompleted="clearCompleted" />
      </transition>
    </div>
  </div>
</template>

<script>
import TodoItem from './TodoItem'
import TodoCheckAll from './TodoCheckAll'
import TodoItemsRemaining from './TodoItemsRemaining'
import TodoFiltered from './TodoFiltered'
import TodoClearCompleted from './TodoClearCompleted'

export default {
  name: 'todo-list',
  components: {
    TodoItem,
    TodoCheckAll,
    TodoItemsRemaining,
    TodoFiltered,
    TodoClearCompleted
  },
  data () {
    return {
      newTodo: '',
      idForTodo: 2,
      filter: 'all',
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
  computed: {
    remaining() {
      return this.todos.filter(todo => !todo.completed).length
    },
    anyRemaining() {
      return this.remaining != 0
    },
    todosFiltered() {
      switch(this.filter) {
        case 'active': return this.todos.filter(todo => !todo.completed)
        case 'completed': return this.todos.filter(todo => todo.completed)
        default: return this.todos
      }
    },
    showClearCompletedButton() {
      return this.todos.filter(todo => todo.completed).length > 0
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
    doneEdit(todo) {
      this.todos.splice(todo.id, 1, todo)
    },
    removeTodo(id) {
      this.todos.splice(id, 1)
    },
    checkAllTodos() {
      this.todos.forEach((todo) => todo.completed = event.target.checked)
    },
    setFilter(filter) {
      this.filter = filter
    },
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed)
    }
  },
}
</script>

<style lang="scss" scoped>
@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");

.todo-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;
  &:focus {
    outline: 0;
  }
}
.extra-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 16px;
  border-top: 1px solid lightgrey;
  padding-top: 14px;
  margin-bottom: 14px;
}

// CSS Transitions
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>