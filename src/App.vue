<template>
		<br>
        <h1 class="Content DuringAuthHide DuringRegisterHide">Meine To-Do-Liste<button id="InformationsButton" @click="showInformations" style="margin-left:2.5em">i</button></h1>
		<!-- <button @click="changeTheme">Theme</button> -->
        <AddToDoButton class="Content DuringAuthHide DuringRegisterHide"  @click="hideInformations" @add-todo-event="addToDoItem"/>
		<br class="Content DuringRegisterHide DuringAuthHide"><br>
		<button @click="logout" id="Logout" class="Content DefaultButton DuringRegisterHide DuringAuthHide">Logout</button>
		<div class="Content DuringRegisterHide" id="authenticateContent">
			Email: &emsp; &emsp; &emsp;<input v-model="LoginUsername"  id="emailInput" type="email">
			<br>
			Password: &emsp;<input v-model="LoginPassword" @keydown="authenticateInput" id="passwordInput"  type="password">
			<br>

			&emsp; &emsp; &emsp;  &emsp; &emsp;
			<button id="LoginLogin" class="DefaultButton" @click="authenticate">Login</button>

			<button id="LoginRegister" class="DefaultButton" @click="showRegister">register</button>
		</div>
		<div class="Content DuringAuthHide" id="registerContent">
			Email: &emsp; &emsp; &emsp;&emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; <input v-model="RegisterUsername" id="RegisterEmail" type="email">
			<br>
			Password: &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp;<input v-model="RegisterPassword" id="RegisterPassword" type="password">
			<br>
			Password bestätigen: &emsp;&emsp; &emsp;<input v-model="RegisterPasswordBest" id="RegisterPasswordBestatigen" type="password">
			<br>
			&emsp; &emsp; &emsp;  &emsp; &emsp;&emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp;
			<button id="RegisterRegister" class="DefaultButton" @click="register">register</button>

			<button id="RegisterLogin" class="DefaultButton" @click="showAuthenticate">Login</button>
		</div>
		<span class="Content DuringTaskHide DuringAuthHide DuringRegisterHide" style="display: none" id="loading">Loading...</span>
		<span class="Content DuringTaskHide DuringAuthHide DuringRegisterHide" id="Informationen"  @click="hideInformations">
			<br>
			Dies ist ein Frontend für den ÜK 294. <br>
			Gemacht wurde dies von  Fabio Kälin.

			<br>
			<br>
			<b>Hinweise:</b>
			<ul>

				<li>Bitte bei Nutzung des Firefox-Browsers beachten, dass manche Dinge (z.B. Scrollbar) nicht richtig angezeigt werden.</li>
				<li>Der Standart-Benutzer ist hugo@m295.local.zli.ch und das Passwort ist Zli123</li>
			</ul>
			<br>
			<br>
		</span>
		<ToDos id="ToDos" class="Content DuringAuthHide DuringRegisterHide" v-bind:todoEntries="todoEntries" @delete-todo-event="deleteToDoItem" @change-Edit-Input="changeEditInput" @edit-todo-event="updateToDoItem"/>
		<br class="Content DuringAuthHide DuringRegisterHide" @click="hideInformations">

		<div class="Content DuringAuthHide DuringRegisterHide" id="AdminContent">
			<button @click="admin" class="DefaultButton" id="AdminButton">Admin</button>
			<input  v-model="sqlInput" id="sqlInput" type="text">
			<pre id="AdminOutput" >{{ this.adminOutput }}</pre>
		</div>
</template>
<script>
	import ToDos from './components/ToDos'
	import AddToDoButton from './components/AddToDoButton'



	// if (document.body.classList == "authenticated"){
	// 	document.body.getElementById("notauthenticate").style.display = "none"
	// }
	document.body.classList.add("unauthenticated")

	// let user = undefined;

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
				adminOutput: "",
				LoginUsername: "",
				LoginPassword: "",
				RegisterUsername: "",
				RegisterPassword: "",
				RegisterPasswordBest: "",
				sqlInput: "",
				user: undefined
			}
		},
		methods: {
			async admin(){
				const url = "http://localhost:4312/v1/admin"
				const response = await fetch(url, {
					method: 'POST',
					body: JSON.stringify({
						"email": this.username,
						"password": this.password,
						"SQL": this.sqlInput
					}),
					headers: {
						// 'authorization': `Bearer ${token}`,
						'Content-Type': 'application/json'
					},
				})
				const json = await response.json()
				this.adminOutput = JSON.stringify(json, null, 2)
			},showRegister() {
				document.getElementById("registerContent").style.display = "block"
				document.getElementById("authenticateContent").style.display = "none"
			},showAuthenticate(){
				document.getElementById("registerContent").style.display = "none"
				document.getElementById("authenticateContent").style.display = "block"
			},Checkpw(pwd, pwdbest){
				if (pwd == pwdbest){
					return true
				} else {
					return false
				}
			},async register(){
				var registerEmail = this.RegisterEmail;
				var registerPassword = this.RegisterPassword;
				var registerPasswordBestatigen = this.RegisterPasswordBest;
				if (this.Checkpw(registerPassword, registerPasswordBestatigen)){
					document.getElementById("loading").style.display = "block"
					let url = "http://localhost:4312/v1/register"
					const response = await fetch(url, {
						method: 'POST',
						body: JSON.stringify({
							"email":registerEmail,	//"hugo@m295.local.zli.ch"		hugo@m295.local.zli.ch
							"password":registerPassword	//"Zli123"
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
						for( const singleContent of document.getElementsByClassName("Content")){
							singleContent.style.display = "none"
						}
						var x = document.getElementsByClassName("DuringAuthHide");
						var y = document.getElementsByClassName("DuringRegisterHide");
						var z = document.getElementById("AdminContent")
						for (const element1 of x) {
							for (const element2 of y) {
								if (element2 == element1){
									if (this.user.admin == 1){
										element1.style.display = "block"
									} else if (element1 != z){
										element1.style.display = "block"
									}
								}
							}
						}
						await this.verify(registerEmail.value, registerPassword.value)
					} else {

						window.alert("Uiuiui etwas ist da aber schiefgelaufen")
					}

				} else {
					window.alert("Uiuiui etwas ist da aber schiefgelaufen")
				}
			},logout(){
				// console.log(this.user)
				this.user = undefined
				this.todoEntries = []
				for( const singleContent of document.getElementsByClassName("Content")){
					singleContent.style.display = "block"
				}
				var x = document.getElementsByClassName("DuringAuthHide");
				for (const element1 of x) {
					element1.style.display = "none"
				}
			},async tasks(){

				var x = document.getElementsByClassName("DuringTaskHide");
				for (const element1 of x) {
					element1.style.display = "none"
				}
				// let email = data.email;

				let id = this.user.id;
				let token = this.user.token
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
				let id = this.user.id;
				let token = this.user.token
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
				this.LoginUsername = ""
				this.LoginPassword = ""
				this.RegisterUsername = ""
				this.registerPassword = ""
				this.RegisterPasswordBest = ""
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
					this.user = json.data
					for( const singleContent of document.getElementsByClassName("Content")){
						singleContent.style.display = "none"
					}
					var x = document.getElementsByClassName("DuringAuthHide");
					var y = document.getElementsByClassName("DuringRegisterHide");
					var z = document.getElementById("AdminContent")
					for (const element1 of x) {
						for (const element2 of y) {
							if (element2 == element1){
								if (this.user.admin == 1){
									element1.style.display = "block"
								} else if (element1 != z){
									element1.style.display = "block"
								}
							}
						}
					}
					this.tasks()
				} else {
					window.alert("Uiuiui etwas ist da aber schiefgelaufen")
				}
			},async addToDoItem(newToDoItem){
				let token = this.user.token
				const url = "http://localhost:4312/v1/tasks"
				const response = await fetch(url, {
					method: 'POST',
					body: JSON.stringify({
						"email":this.user.email,
						"description": newToDoItem.title
					}),
					headers: {
						'authorization': `Bearer ${token}`,
						'Content-Type': 'application/json'
					}
				})
				const json = await response.json()
				if (json.code == 200){
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
					let token = this.user.token
					const url = `http://localhost:4312/v1/tasks/${toDoId}`
					const response = await fetch(url, {
						method: 'POST',
						body: JSON.stringify({
							"email":this.user.email,
							"description": new_content
						}),
						headers: {
							'authorization': `Bearer ${token}`,
							'Content-Type': 'application/json'
						}
					})
					const json = await response.json()
					if (json.code == 200){
						this.fetchUpdate()
					} else {
						window.alert("Uiuiui etwas ist da aber schiefgelaufen")
					}
				}
			},async deleteToDoItem(toDoId){
				let token = this.user.token
				const url = `http://localhost:4312/v1/tasks/${toDoId}`
				const response = await fetch(url, {
					method: 'DELETE',
					body: JSON.stringify({
						"email":this.user.email,
					}),
					headers: {
						'authorization': `Bearer ${token}`,
						'Content-Type': 'application/json'
					}
				})
				const json = await response.json()
				if (json.code == 200){
					this.tasks()
				} else {
					window.alert("Uiuiui etwas ist da aber schiefgelaufen")
				}
			},async authenticate() {
				this.verify(this.LoginUsername, this.LoginPassword)
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