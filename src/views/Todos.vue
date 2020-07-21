<template>
    <div>
        <h2>List</h2>
        <router-link to="/">Home</router-link>
        <AddTodo
            @add-todo="addTodo"
        />
        <select v-model="filter">
            <option value="all">All</option>
            <option value="completed">Completed</option>
            <option value="not-completed">Not Completed</option>
        </select>
        <hr />
        <Loader 
            v-if="loading"
        />
        <TodoList 
            v-else-if="filteredItems.length"
            v-bind:todos="filteredItems"
            @remove-todo="removeTodo"
        />
        <p v-else>No items!</p>
    </div>
</template>

<script>
import Loader from "@/components/Loader";
import TodoList from "@/components/TodoList";
import AddTodo from "@/components/AddTodo";
export default {
    name: "App",
    data() {
        return {
            todos: [],
            loading: true,
            filter: 'all',
        }
    },
    mounted() {
        fetch('https://jsonplaceholder.typicode.com/todos?_limit=5')
            .then(response => response.json())
            .then(json => {
                setTimeout(() => {
                    this.todos = json;
                    this.loading = false;
                }, 500);
            })
    },
    computed: {
        filteredItems() {
            switch (this.filter) {
                case 'all':
                    return this.todos;
                    break;
            
                case 'completed':
                    return this.todos.filter(t => t.completed);
                    break;

                case 'not-completed':
                    return this.todos.filter(t => !t.completed);
                    break;
            }
        }
    },
    methods: {
        removeTodo(id) {
            this.todos = this.todos.filter(t => t.id !== id);
        },
        addTodo(data) {
            this.todos.push(data);
        }
    },
    components: {
        TodoList, AddTodo, Loader
    },
};
</script> 

<style scoped>
select {
    margin-top: 1rem;
}
</style>