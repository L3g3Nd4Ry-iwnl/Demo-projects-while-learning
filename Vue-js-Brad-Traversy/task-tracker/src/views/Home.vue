<template>
    <div>
        <AddTask v-show="showAddTask" @add-task="AddTask" @hide-add-task="toggleAddTask"/>
        <Tasks @delete-task="deleteTask" @toggle-reminder="toggleReminder" :tasks="tasks" />
    </div>
</template>

<script>
import Tasks from '../components/Tasks.vue';
import AddTask from '../components/AddTask.vue';

export default {
    name: 'Home',
    components: {
        AddTask,
        Tasks
    },
    data() {
        return{
            tasks: []
        }
    },
    props: {
        showAddTask: Boolean,
        toggleAddTask: Boolean
    },
    async created(){
    this.tasks= await this.fetchTasks();
  },
  methods: {
    async deleteTask(id){
      if(confirm('Are you sure you want to delete this event?')){
        const res = await fetch(`/api/tasks/${id}`,{
          method: 'DELETE'
        })
        res.status === 200 ? this.tasks = this.tasks.filter((task) => task.id !== id) : alert('Some error in json-server');
      }
    },
    async toggleReminder(id){
      const taskToToggle = await this.fetchTask(id);
      const updTask = {...taskToToggle, reminder: !taskToToggle.reminder}

      const res = await fetch(`api/tasks/${id}`,{
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json',
        }, 
        body: JSON.stringify(updTask)
      })

      const data = await res.json()

      this.tasks = this.tasks.map(
        (task) => task.id === id ? {...task, reminder: data} : task
      )
    },
    async AddTask(task){
      const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify(task)
      })

      const data = await res.json();

      this.tasks = [...this.tasks, data];
    },
    async fetchTasks(){
      const res = await fetch('api/tasks');
      const data = await res.json();

      return data;
    },
    async fetchTask(id){
      const res = await fetch(`api/tasks/${id}`);
      const data = await res.json();

      return data;
    }
  }
}
</script>