<template>
        <h1>Meine To-Do-Liste</h1>
		<ToDos v-bind:todoEntries="todoEntries" @delete-todo-event="deleteToDoItem" @edit-todo-event="editToDoItem"/>
		<br>
        <AddToDoButton @add-todo-event="addToDoItem"/>

</template>
<script>
	console.log("app exec")
	import ToDos from './components/ToDos'
	import AddToDoButton from './components/AddToDoButton'
    export default {
		name: 'App',
		components: {
			ToDos,
			AddToDoButton
		},
		data() {
			return {
				todoEntries: [
					{
						id: 1,
						title: 'Infos sammeln',
						completed: false
					},
					{
						id: 2,
						title: 'Test Projekt erstellen',
						completed: false
					},
					{
						id: 3,
						title: 'Blog Beitrag schreiben',
						completed: false
					},
				],
			}
		},
		methods: {
			addToDoItem(newToDoItem) {
				this.todoEntries = [...this.todoEntries, newToDoItem];
				console.log(this.todoEntries)
			},
			deleteToDoItem(toDoId){
				this.todoEntries = this.todoEntries.filter(item => item.id !== toDoId)
			},

            editToDoItem(toDoId){
				console.log(this.todoEntries[toDoId - 1].title)
			}
		}
	}

	console.log("app end")
</script>


<style scoped>
	@import "../style.css"
</style>