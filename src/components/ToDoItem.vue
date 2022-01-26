<template>
    <div class="todo-item"  v-bind:class="{'completed':this.todo.completed}">
            <input type="checkbox" @change="markCompleted" value="✓">  {{todo.title}}
            <button @click="$emit('delete-todo-event', todo.id)">Löschen</button>
    </div>
</template>
<script>
    console.log("item exec")
    export default {
        name: "TodoItem",
        props: ["todo_prop"],
        data() {
            return {
                todo: this.todo_prop
            }
        },
        methods: {
            markCompleted() {
                console.log(this.todo.completed)
                this.todo.completed = !this.todo.completed
                console.log(this.todo.completed)
            },
            deleteToDoItem(toDoId){
                this.todoEntries = this.todoEntries.filter(item => item.id !== toDoId)
            }
        }
    }
    console.log("item end")
</script>
<style>
.completed{
    background-color: green;
}
.completed {
    text-decoration: line-through;
}
</style>