<template>
        <h1 class="DuringAuthHide DuringRegisterHide">Meine To-Do-Liste<button id="InformationsButton" @click="showInformations" style="margin-left:2.5em">i</button></h1>
		<!-- <button @click="changeTheme">Theme</button> -->
        <AddToDoButton class="DuringAuthHide DuringRegisterHide"  @click="hideInformations" @add-todo-event="addToDoItem"/>
		<br class="DuringRegisterHide DuringAuthHide"><br>
		<div class="DuringRegisterHide" id="authenticateContent">
			Email<input  id="emailInput" type="text">
			<br>
			Password<input  id="passwordInput" type="password">
			<br>
			<button @click="authenticate">Login</button>
			<!-- <button  @click="tasks" >Tasks</button> -->

			<button @click="showRegister">register</button>
		</div>
		<div class="DuringAuthHide" id="registerContent">
			Email: &emsp; &emsp; &emsp;&emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; <input  id="RegisterEmail" type="text">
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
		<ToDos class="DuringAuthHide DuringRegisterHide" v-bind:todoEntries="todoEntries" @delete-todo-event="deleteToDoItem" @change-Edit-Input="changeEditInput" @edit-todo-event="editToDoItem"/>
		<br class="DuringAuthHide DuringRegisterHide" @click="hideInformations">

</template>
<script>
	console.log("app exec")
	import ToDos from './components/ToDos'
	import AddToDoButton from './components/AddToDoButton'

	// if (document.body.classList == "authenticated"){
	// 	console.log("asdf")
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
				console.log(pwd)
				console.log(pwdbest)
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
					console.log(registerEmail.value)
					let email = await registerEmail.value
					let response = await fetch(url, {
						method: 'POST',
						body: JSON.stringify({
							"email":email,	//"hugo@m295.local.zli.ch"		hugo@m295.local.zli.ch
							"password":registerPassword.value	//"Zli123"
						}),
						headers: {
							'Content-Type': 'application/json'
						}
					})
					console.log(response)

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
					// let json = await response.json()
					// user = json.data
					this.verify(registerEmail.value, registerPassword.value)
					this.tasks()
				} else {
					window.alert("Fehlgeschlagen bitte von neuem Beginnen")
				}
			},addToDoItem(newToDoItem) {
				this.todoEntries = [...this.todoEntries, newToDoItem];
			},deleteToDoItem(toDoId){
				this.todoEntries = this.todoEntries.filter(item => item.id !== toDoId)
			},editToDoItem(toDoId){
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
									fulltodo.description = new_content
								}
							}
							element.value = ""
						}
					}
					this.changeEditInput(toDoId)
				}

			},async tasks(){
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
				// console.log("tasks");
				const url = "http://localhost:4312/v1/tasks"

				const response = await fetch(url, {
					method: 'GET',
					// body: JSON.stringify({
					// 	"email":"hugo@m295.local.zli.ch",
					// 	"password":"Zli123"
					// }),
					headers: {
						'authorization': `Bearer ${token}`,
						'Content-Type': 'application/json'
					},
					uid: id
				})
				const json = await response.json()
				// console.log(json)
				// console.log(json.data.token)

				// function jsonInToDoEntries(item, index, arr) {
				// 	let object = new Object();
				// 	console.log(item)
				// 	object.description = item.description
				// 	object.id = item.id
				// 	this.todoEntries.push(object)
				// 	console.log(index, arr)
				// }
				this.todoEntries = []
				// json.data.forEach(jsonInToDoEntries(this))
				for (const item of json.data){
					let object = new Object();
					object.description = item.description
					object.id = item.id
					this.todoEntries.push(object)
				}
				// let token = json.data.token
				// return token;
				console.log("Tasks end")
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
				user = json.data

				this.tasks()

			},async authenticate() {
				var email = document.getElementById("emailInput").value;
				var password = document.getElementById("passwordInput").value;
				this.verify(email, password)

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

	// document.getElementById("authenticateContent").style.display = "display"

	console.log("app end")
</script>


<style scoped>
	@import "../style.css"
</style>