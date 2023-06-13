<template>
    <AddTask v-show="showAddTask" @add-task="addTask" />
    <Tasks @delete-task="deleteTask" :tasks="tasks" />
</template>

<script>
import Tasks from '../components/Tasks'
import AddTask from '../components/AddTask'
export default {
    name: 'Home',
    props: {
        showAddTask: Boolean,
    },
    components: {
        Tasks,
        AddTask,
    },
    data() {
        return {
            tasks: [],
        }
    },
    methods: {
    async addTask(task) {
      const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
          
        },
        body: JSON.stringify(task),
      })
      const data = await res.json()

      this.tasks = [...this.tasks, data]
    },
    async deleteTask(id){
    if (confirm('Are you sure?')) {
      const res = await fetch(`api/tasks/${id}`, {
        method: 'DELETE',
      })
       res.status === 200 ? 
       (this.tasks = this.tasks.filter(
       (task) => task.id !== id)) : alert('Error deleting...')
      }
    },
    async fetchtasks() {
      const res = await fetch('api/tasks')

      const data = await res.json()

      return data
    },
    async fetchtask(id) {
      const res = await fetch(`api/tasks/${id}`)

      const data = await res.json()

      return data
    },
  },
  async created() {
    this.tasks = await this.fetchtasks()
  }
}
</script>