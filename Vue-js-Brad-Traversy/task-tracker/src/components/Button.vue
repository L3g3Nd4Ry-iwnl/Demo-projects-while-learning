<template>
    <button @click="showAddTask" class="btn" :style="{ background:color }" >{{ name }}</button>
</template>

<script>

export default {
    name: 'Button',
    props: {
        name: String,
        color: String
    },
    methods:{
        showAddTask(){
            this.$emit("btn-click");
        }
    }
}
</script>
