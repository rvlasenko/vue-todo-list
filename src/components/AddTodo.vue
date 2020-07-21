<template>
    <form action="" @submit.prevent="onSubmit">
        <input type="text" v-model="title">
        <button type="submit">Create</button>
    </form>
</template>

<script>
export default {
    data() {
        return {
            title: '',
        }
    },
    methods: {
        onSubmit() {
            if (this.title.trim()) {
                var newTodo = {
                    id: Date.now(),
                    title: this.title,
                    completed: false,
                };

                this.$emit('add-todo', newTodo);
                this.title = '';
            }
        }
    }
}
</script>

<style scoped>
button {
    margin-left: 1rem;
}
</style>