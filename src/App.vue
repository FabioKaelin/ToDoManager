<template>
        <h1 class="DuringAuthHide DuringRegisterHide">Meine To-Do-Liste<button id="InformationsButton" @click="showInformations" style="margin-left:2.5em">i</button></h1>
		<!-- <button @click="changeTheme">Theme</button> -->
        <AddToDoButton class="DuringAuthHide DuringRegisterHide"  @click="hideInformations" @add-todo-event="addToDoItem"/>
		<br class="DuringRegisterHide DuringAuthHide"><br>
		<div class="DuringRegisterHide" id="authenticateContent">
			Email: &emsp; &emsp; &emsp;<input  id="emailInput" type="email">
			<br>
			Password: &emsp;<input @keydown="authenticateInput" id="passwordInput"  type="password">
			<br>
			<button @click="authenticate">Login</button>
			<!-- <button  @click="tasks" >Tasks</button> -->

			<button @click="showRegister">register</button>
		</div>
		<div class="DuringAuthHide" id="registerContent">
			Email: &emsp; &emsp; &emsp;&emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; <input  id="RegisterEmail" type="email">
			<br>
			Password: &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp;<input  id="RegisterPassword" type="password">
			<br>
			Password bestätigen: &emsp;&emsp; &emsp;<input  id="RegisterPasswordBestatigen" type="password">
			<br>
			&emsp; &emsp; &emsp; &emsp; &emsp; &emsp;&emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp;<button @click="register">register</button>

			<button @click="showAuthenticate">Login</button>
		</div>
		<span style="display: none" id="loading">Loading...</span>
		<span id="Informationen"  @click="hideInformations">
			<br>
			Dies ist ein Forntend für den ÜK 294. <br>
			Gemacht wurde dies von Fabio Kälin.

			<br>
			<br>
			<b>Hinweise:</b>
			<ul>

				<li>Bitte bei Nutzung des Firefox-Browsers beachten das manche Dinge (z.B. Scrollbar) nicht richtig angezeigt werden.</li>
				<li>Der Standart-Benutzer ist hugo@m295.local.zli.ch und das Passwort ist Zli123</li>
			</ul>
			<br>
			<br>
			</span>
		<ToDos id="ToDos" class="DuringAuthHide DuringRegisterHide" v-bind:todoEntries="todoEntries" @delete-todo-event="deleteToDoItem" @change-Edit-Input="changeEditInput" @edit-todo-event="updateToDoItem"/>
		<br class="DuringAuthHide DuringRegisterHide" @click="hideInformations">

</template>
<script>
	import ToDos from './components/ToDos'
	import AddToDoButton from './components/AddToDoButton'



	// if (document.body.classList == "authenticated"){
	// 	document.body.getElementById("notauthenticate").style.display = "none"
	// }
	document.body.classList.add("unauthenticated")

	let user = undefined;

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
						description: 'Infos sammeln',
						// completed: false
					},
					{
						id: 2,
						description: 'Test Projekt erstellen',
						// completed: false
					},
					{
						id: 3,
						description: 'Blog Beitrag schreiben',
						// completed: false
					},
				],
			}
		},
		methods: {
			showRegister() {
				document.getElementById("registerContent").style.display = "block"
				document.getElementById("authenticateContent").style.display = "none"
			},showAuthenticate(){
				document.getElementById("registerContent").style.display = "none"
				document.getElementById("authenticateContent").style.display = "block"
			},Checkpw(pwd, pwdbest){
				if (pwd == pwdbest){
					// window.location.replace("./index.html");
					// window.alert("Index")
					return true
				} else {
					// window.location.replace("./anmeldungFehlgeschlagen.html");
					window.alert("Fehlgeschlagen")
					return false
				}
				// window.alert("Hallo")
			},async register(){
				var registerEmail = document.getElementById("RegisterEmail");
				var registerPassword = document.getElementById("RegisterPassword");
				var registerPasswordBestatigen = document.getElementById("RegisterPasswordBestatigen");
				if (this.Checkpw(registerPassword.value, registerPasswordBestatigen.value)){
					document.getElementById("loading").style.display = "block"
					let url = "http://localhost:4312/v1/register"
					let email = await registerEmail.value
					const response = await fetch(url, {
						method: 'POST',
						body: JSON.stringify({
							"email":email,	//"hugo@m295.local.zli.ch"		hugo@m295.local.zli.ch
							"password":registerPassword.value	//"Zli123"
						}),
						headers: {
							'Content-Type': 'application/json'
						}
					})

					// url = "http://localhost:4312/v1/authenticate"
					// response = await fetch(url, {
					// 	method: 'POST',
					// 	body: JSON.stringify({
					// 		"email":registerEmail.value,	//"hugo@m295.local.zli.ch"		hugo@m295.local.zli.ch
					// 		"password":registerPassword.value	//"Zli123"
					// 	}),
					// 	headers: {
					// 		'Content-Type': 'application/json'
					// 	}
					// })
					let json = await response.json()
					//user = json.data
					if (json.code == 200) {
						await this.verify(registerEmail.value, registerPassword.value)
					} else {

						window.alert("Uiuiui etwas ist da aber schiefgelaufen")
					}

				} else {
					window.alert("Uiuiui etwas ist da aber schiefgelaufen")
				}
			},/*addToDoItem(newToDoItem) {
				this.todoEntries = [...this.todoEntries, newToDoItem];
			},*/
			async tasks(){
				// let email = data.email;
				document.getElementById("authenticateContent").style.display = "none"
				document.getElementById("registerContent").style.display = "none"
				document.getElementById("loading").style.display = "none"
				var x = document.getElementsByClassName("DuringAuthHide");
				var y = document.getElementsByClassName("DuringRegisterHide");
				for (const element1 of x) {
					for (const element2 of y) {
						if (element2 == element1)
						element1.style.display = "block"
					}
				}
				let id = user.id;
				let token = user.token
				const url = "http://localhost:4312/v1/tasks"

				const response = await fetch(url, {
					method: 'GET',
					headers: {
						'authorization': `Bearer ${token}`,
						'Content-Type': 'application/json'
					},
					uid: id
				})
				const json = await response.json()
				this.todoEntries = []
				for (const item of json.data){
					let object = new Object();
					object.description = item.description
					object.id = item.id
					this.todoEntries.push(object)
				}
			},async fetchUpdate(){
				// let email = data.email;
				document.getElementById("authenticateContent").style.display = "none"
				document.getElementById("registerContent").style.display = "none"
				document.getElementById("loading").style.display = "none"
				var x = document.getElementsByClassName("DuringAuthHide");
				var y = document.getElementsByClassName("DuringRegisterHide");
				for (const element1 of x) {
					for (const element2 of y) {
						if (element2 == element1)
						element1.style.display = "block"
					}
				}
				let id = user.id;
				let token = user.token
				const url = "http://localhost:4312/v1/tasks"

				const response = await fetch(url, {
					method: 'GET',
					headers: {
						'authorization': `Bearer ${token}`,
						'Content-Type': 'application/json'
					},
					uid: id
				})
				const json = await response.json()
				x = document.getElementsByClassName("todoitem");

				for (const task of json.data){
					for (const element of x) {
						if(element.id == task.id){
							for (const titleelement of document.getElementsByClassName("Taskname")){
								if (titleelement.parentNode.id == task.id){
									titleelement.innerText = task.description
								}
							}
						}
					}
				}
			},async verify(email, password){
				const url = "http://localhost:4312/v1/authenticate"
				const response = await fetch(url, {
					method: 'POST',
					body: JSON.stringify({
						"email":email,	//"hugo@m295.local.zli.ch"		hugo@m295.local.zli.ch
						"password":password	//"Zli123"
					}),
					headers: {
						'Content-Type': 'application/json'
					}
				})
				const json = await response.json()
				if (json.code == 200) {
					user = json.data
					this.tasks()
				} else {
					window.alert("Uiuiui etwas ist da aber schiefgelaufen")
				}
			},async addToDoItem(newToDoItem){
				let token = user.token
				const url = "http://localhost:4312/v1/tasks"
				const response = await fetch(url, {
					method: 'POST',
					body: JSON.stringify({
						"email":user.email,
						"description": newToDoItem.title
					}),
					headers: {
						'authorization': `Bearer ${token}`,
						'Content-Type': 'application/json'
					}
				})
				const json = await response.json()
				if (json.code == 200){
					window.alert("Erfolgreich hinzugefügt")
					this.tasks()
				} else {
					window.alert("Uiuiui etwas ist da aber schiefgelaufen")
				}

			},async updateToDoItem(toDoId){
				if(event.keyCode == 13) {
					var x = document.getElementsByClassName("editInput");
					let new_content

					for (const element of x) {
						if(toDoId == element.parentElement.id){
							new_content = element.value
							element.value = ""
						}
					}
					this.changeEditInput(toDoId)
					let token = user.token
					const url = `http://localhost:4312/v1/tasks/${toDoId}`
					const response = await fetch(url, {
						method: 'POST',
						body: JSON.stringify({
							"email":user.email,
							"description": new_content
						}),
						headers: {
							'authorization': `Bearer ${token}`,
							'Content-Type': 'application/json'
						}
					})
					const json = await response.json()
					if (json.code == 200){
						window.alert("Erfolgreich geupdatet")
						this.fetchUpdate()
					} else {
						window.alert("Uiuiui etwas ist da aber schiefgelaufen")
					}
				}
			},async deleteToDoItem(toDoId){
				let token = user.token
				const url = `http://localhost:4312/v1/tasks/${toDoId}`
				const response = await fetch(url, {
					method: 'DELETE',
					body: JSON.stringify({
						"email":user.email,
					}),
					headers: {
						'authorization': `Bearer ${token}`,
						'Content-Type': 'application/json'
					}
				})
				const json = await response.json()
				if (json.code == 200){
					window.alert("Erfolgreich gelöscht")
					this.tasks()
				} else {
					window.alert("Uiuiui etwas ist da aber schiefgelaufen")
				}
			},async authenticate() {
				var email = document.getElementById("emailInput").value;
				var password = document.getElementById("passwordInput").value;
				this.verify(email, password)
			},async authenticateInput() {
				if(event.keyCode == 13){
					var email = document.getElementById("emailInput").value;
					var password = document.getElementById("passwordInput").value;
					this.verify(email, password)
				}
			},changeEditInput(toDoId){
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

            },showInformations(){
				let info = document.getElementById("Informationen")
				if (info.style.display == "none") {
					info.style.display = "unset";
				} else if (info.style.display == "unset"){
					info.style.display = "none";
				} else {
					info.style.display = "unset";
				}
			},hideInformations() {
				document.getElementById("Informationen").style.display = "none";
			},changeTheme() {
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

	// document.getElementById("authenticateContent").style.display = "display"


</script>


<style scoped>
	@import "../style.css"
</style>