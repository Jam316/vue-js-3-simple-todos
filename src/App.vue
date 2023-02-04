<script>
import { ref, computed, watchEffect } from 'vue'
import TodoItem from './components/TodoItem.vue'
import CreateTodo from './components/CreateTodo.vue'

export default {
  components: {
    TodoItem,
    CreateTodo
  },
  setup() {
    let updatedTodos

    const todos = ref([])
    const todoEditing = ref(null)

    const onSubmit = (newTodo) => {
      if (newTodo.text.length > 0) {
        todos.value.push(newTodo)
        saveData(todos.value)
      } else {
        alert('Enter todo!')
      }
    }

    const toggleComplete = (todoId) => {
      const updatedTodos = todos.value.map(todo =>
        todo.id === todoId ? { ...todo, completed: !todo.completed } : todo
      )
      todos.value = updatedTodos
      saveData(todos.value)
    }

    const deleteTodo = (todoId) => {
      updatedTodos = todos.value.filter(todo => todo.id !== todoId)
      todos.value = updatedTodos
      saveData(todos.value)
    }

    const enableEdit = (todoId) => todoEditing.value = todoId

    const updateTodo = (todoId, editingText) => {
      const updatedTodos = todos.value.map(todo =>
        todo.id === todoId ? { ...todo, text: editingText } : todo
      )
      alert(`${editingText} updated!`)
      todos.value = updatedTodos
      todoEditing.value = null
      saveData(todos.value)
    }

    const noItems = computed(() => todos.value.length ? `${todos.value.length} Item(s)` : 'No todos')

    const saveData = todos => {
      localStorage.setItem("todos", JSON.stringify(todos))
    }

    watchEffect(() => {
      const json = localStorage.getItem("todos")
      const loadedTodos = JSON.parse(json)
      if (loadedTodos) {
        todos.value = loadedTodos
      }
    })

    return {
      todos,
      todoEditing,
      noItems,
      onSubmit,
      toggleComplete,
      deleteTodo,
      enableEdit,
      updateTodo
    }
  }
}


</script>

<template>
  <div id="todo-list">
    <h1>Todo List</h1>
    <span>{{ noItems }}</span>
    <CreateTodo @onCreate="onSubmit" />
    <TodoItem :todos="todos" :todoEditing="todoEditing"
      @onComplete="toggleComplete" @onDelete="deleteTodo" @onEdit="enableEdit"
      @onUpdate="updateTodo" />
  </div>
</template>
