<template>
    <header>
        <h1>{{ title }}</h1>
        <Button v-show="homePage" :name="show ? 'Close' : 'Add Task'" :color="show ? 'red' : 'green'" @btn-click="showAddTask"  />  
    </header>
</template>

<script>
import Button from './Button';

export default {
    name: 'Header',
    props: {
        title: String,
        show: Boolean
    },
    components: {
        Button
    },
    methods: {
        showAddTask(){
            this.$emit('show-add-task');
        }
    },
    computed:{
        homePage(){
            return this.$route.path === "/" ? true : false;
        }
    }
}
</script>

<style scoped>
    header{
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
    }
</style>