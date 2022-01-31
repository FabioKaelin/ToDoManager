<template>
        <h1  >Meine To-Do-Liste<button id="InformationsButton" @click="showInformations" style="margin-left:2.5em">i</button> <button @click="changeTheme">Theme</button> </h1>
		<span id="Informationen"  @click="hideInformations">
			<br>
			Dies ist ein Forntend für den ÜK 294. <br>
			Gemacht wurde dies von Fabio Kälin.

			<br>
			<br>
			<b>Hinweise:</b>
			<ul>

				<li>Bitte bei Nutzung des Firefox-Browsers beachten das manche Dinge (z.B. Scrollbar) nicht richtig angezeigt werden.</li>
			</ul>
			<br>
			<br>
			</span>
		<ToDos v-bind:todoEntries="todoEntries" @delete-todo-event="deleteToDoItem" @change-Edit-Input="changeEditInput" @edit-todo-event="editToDoItem"/>
		<br @click="hideInformations">
        <AddToDoButton  @click="hideInformations" @add-todo-event="addToDoItem"/>

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

            },
			showInformations(){
				console.log(document.getElementById("Informationen"))
				let info = document.getElementById("Informationen")
				if (info.style.display == "none") {
					info.style.display = "unset";
				} else if (info.style.display == "unset"){
					info.style.display = "none";
				} else {
					info.style.display = "unset";
				}
			},
			hideInformations() {
				document.getElementById("Informationen").style.display = "none";
			},
			changeTheme() {
				console.log(document.scrollingElement.style)
				if (document.body.style.color == "rgb(28, 82, 83)") {// if lighttheme then Darktheme
					document.body.style.color = "rgb(58, 145, 148)";
					document.body.style.backgroundColor = "rgb(16, 40, 66)"
				} else if (document.body.style.color == "rgb(58, 145, 148)"){//if Darktheme then Lighttheme
					document.body.style.color = "rgb(28, 82, 83)";
					document.body.style.background = "rgb(127, 255, 212"

				}
			}
		}
	}

	console.log("app end")
</script>


<style scoped>
	@import "../style.css"
</style>