<template>
    <div>
        <p v-for="todo in todos" :key="todo.id">{{ todo.title }}</p>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data() {
        return {
            todos: {}
        }
    },
    methods: {
        getTodos() {
            axios.get('https://jsonplaceholder.typicode.com/todos')
                .then(res => {
                    this.todos = res.data;
                })
                .catch(err => {

                });
        }
    },
    created() {
        this.getTodos();
    }
}
</script>