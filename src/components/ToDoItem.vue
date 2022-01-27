<template>
    <div class="todoitem"  @click="markCompleted" v-bind:class="{'completed':this.todo.completed}">
        <span class="Taskname">{{todo.title}}</span>

        <button class="papierkorbButton" @click="$emit('delete-todo-event', todo.id)">
            <svg height="20" width="20">
                <polyline points="5,7.5 5,15 7.5,17.5 12.5,17.5 15,15 15,7.5"
                style="fill:none;stroke:black;stroke-width:1.5" />
                <polyline points="5,5 15,5 16,5.5 16.5,6.125 16,7 15,7.5 5,7.5 4,7 3.5,6.125 4,5.5 5,5 15,5"
                style="fill:none;stroke:black;stroke-width:1.5" />
                <polyline points="7.5,5 8.5625,3.0625  10,2.5 11.4375,3.0625 12.5,5"
                style="fill:none;stroke:black;stroke-width:1.5" />
            </svg>
        </button>
            <!-- <svg height="40" width="40">
                <polyline points="10,15 10,30 15,35 25,35 30,30 30,15"
                style="fill:none;stroke:black;stroke-width:3" />
                <polyline points="10,10, 30,10 32,11 33,12.5 32,14 30,15 10,15 8,14 7,12.5 8,11 10,10 30,10"
                style="fill:none;stroke:black;stroke-width:3" />
                <polyline points="15,10 17.125,6.125  20,5 22.875,6.125 25,10"
                style="fill:none;stroke:black;stroke-width:3" />
            </svg> -->
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

<style scoped>
	@import "../../style.css"
</style>