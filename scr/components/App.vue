<template>
    <div>
      <h1>Lista de Tareas</h1>
      <form @submit.prevent="addTask">
        <input type="text" v-model="newTask" placeholder="Nueva tarea" required>
        <button type="submit">Agregar</button>
      </form>
      <ul>
        <li v-for="task in tasks" :key="task.id">
          {{ task.title }}
          <button @click="deleteTask(task.id)">Eliminar</button>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        tasks: [],
        newTask: ''
      }
    },
    mounted() {
      this.fetchTasks()
    },
    methods: {
      fetchTasks() {
        this.$http.get('/api/tasks')
          .then(response => {
            this.tasks = response.data
          })
          .catch(error => {
            console.log(error)
          })
      },
      addTask() {
        this.$http.post('/api/tasks', { title: this.newTask })
          .then(response => {
            this.tasks.push(response.data)
            this.newTask = ''
          })
          .catch(error => {
            console.log(error)
          })
      },
      deleteTask(taskId) {
        this.$http.delete(`/api/tasks/${taskId}`)
          .then(() => {
            this.tasks = this.tasks.filter(task => task.id !== taskId)
          })
          .catch(error => {
            console.log(error)
          })
      }
    }
  }
  </script>