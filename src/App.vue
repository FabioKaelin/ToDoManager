<template>
        <h1>Meine To-Do-Liste</h1>
		<ToDos v-bind:todoEntries="todoEntries" @delete-todo-event="deleteToDoItem" @change-Edit-Input="changeEditInput" @edit-todo-event="editToDoItem"/>
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
			},
			deleteToDoItem(toDoId){
				this.todoEntries = this.todoEntries.filter(item => item.id !== toDoId)
			},

            editToDoItem(toDoId){
				if(event.keyCode == 13) {
					let id;
					for (const element of this.todoEntries) {
						if(toDoId == element.id){
							id = element.id

						}
					}
					var x = document.getElementsByClassName("editInput");
					for (const element of x) {
						if(id == element.parentElement.id){
							let new_content = element.value
							for (const fulltodo of this.todoEntries){
								if (id == fulltodo.id){
									fulltodo.title = new_content
								}
							}
							element.value = ""
						}
					}
					this.changeEditInput(toDoId)
				}

			},
			changeEditInput(toDoId){
                let id;
				for (const element of this.todoEntries) {
					if(toDoId == element.id){
						id = element.id

					}
				}
                var x = document.getElementsByClassName("editInput");
				for (const element of x) {
					if(id == element.parentElement.id){
						if (element.style.display == "none") {
							element.style.display = "unset";
						} else if (element.style.display == "unset"){
							element.style.display = "none";
						} else {
							element.style.display = "unset";
						}
					}
				}

            }
		}
	}

	console.log("app end")
</script>


<style scoped>
	@import "../style.css"
</style>