<template>
    <div id="TaskListe">
        <ToDoItem  v-for="todo in todoEntries" v-bind:id="todo.id" v-bind:key="todo.id"  v-bind:todo_prop="todo"  @delete-todo-event="$emit('delete-todo-event', todo.id)" @change-Edit-Input="$emit('change-Edit-Input', todo.id)" @edit-todo-event="$emit('edit-todo-event', todo.id)" />
    </div>
</template>
<script>
    console.log("todos exec")
    import ToDoItem from "./ToDoItem";
    export default {
        name: 'ToDos',
        components: {
            ToDoItem
        },
        props: [
            "todoEntries"
        ]
    }
    console.log("todos end")
</script>
