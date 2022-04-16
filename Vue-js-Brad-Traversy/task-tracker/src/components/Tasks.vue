<template>
    <div>
        <div :key="task.id" v-for="task in tasks">
            <Task :task="task" @delete-task="deleteTask" @toggle-reminder="toggleReminder" />
        </div>
    </div>
</template>

<script>
    import Task from './Task.vue';

    export default {
        name: 'Tasks',
        props: {
            tasks: Array
        },
        components: {
            Task
        },
        methods: {
            deleteTask(id){
                this.$emit('delete-task', id);
            },
            toggleReminder(id){
                this.$emit('toggle-reminder', id);
            }
        }
    }
</script>