<template>
    <div id="TaskListe">
        <ToDoItem  v-for="todo in todoEntries" v-bind:id="todo.id" v-bind:key="todo.id"  v-bind:todo_prop="todo"  @delete-todo-event="$emit('delete-todo-event', todo.id)" @change-Edit-Input="$emit('change-Edit-Input', todo.id)" @edit-todo-event="$emit('edit-todo-event', todo.id)" />
    </div>
        <svg height="100" width="100">
            <polyline points="14,72 74,8 74.5,10 75,11 78,12 79,12 19,78 11,81 14.3,72.2 19,78"
            style="fill:none;stroke:black;stroke-width:1.5" />
            <polyline points="49,35 54,40 52,42 47,37"
            style="fill:none;stroke:black;stroke-width:1.5" />
            <polyline points="74,7 77,8 79,9 79.5,11.5"
            style="fill:none;stroke:black;stroke-width:1.5" />
            <polyline points="69.7,13 66,13 64,14 50,29"
            style="fill:none;stroke:black;stroke-width:1.5" />
        </svg>
</template>
<script>
    console.log("todoLineItem exec")
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
