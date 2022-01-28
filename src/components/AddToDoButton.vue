//AddToDoButton.vue
<template>
    <div id="Add Task">
        <form @submit="addToDo">
            <input class="AddInput" type="text" v-model="title" name="title">
            <button class="addButton" type="submit">Hinzufügen</button>
        </form>
    </div>
</template>
<script>
    import { v4 as uuidv4 } from 'uuid';
    export default {
        name: 'AddToDoButton',
        data() {
            return {
                title: ''
            }
        },
        methods: {
            addToDoItem(newToDoItem) {
                this.todoEntries = [...this.todoEntries, newToDoItem];
                console.log(this.todoEntries)
            },
            addToDo(event){
                event.preventDefault();
                const newToDoObject = {
                    id: uuidv4(),
                    title: this.title,
                    completed: false
                }
                this.$emit("add-todo-event", newToDoObject);
                this.title= '';

            }
        }
    }

</script>
<style scoped>
	@import "../../style.css"
</style>