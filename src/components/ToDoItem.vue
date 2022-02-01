<template>
    <div class="todoitem" >
        <!-- v-bind:class="{'completed':this.todo.completed}" -->
        <span   class="Taskname">{{todo.description}}</span>
        <!-- @click="markCompleted" -->
        <!-- @blur="$emit('edit-todo-event', todo.id)" -->
        <input type="text"  @keydown="$emit('edit-todo-event', todo.id)" class="editInput">
        <!-- <input type="button" class="showInputButton" value="edit"  @click="$emit('change-Edit-Input', todo.id)"> -->
        <!-- @blur="editToDoItem" -->

        <button class="showInputButton" @click="$emit('change-Edit-Input', todo.id)">
            <svg height="20" width="20">
                <polyline points="2.8,14.4 14.8,1.6 14.88,2 15,2.2 15.6,2.4 15.8,2.4 3.8,15.6 2.2,16.2 2.86,14.44 3.8,15.6"
                style="fill:none;stroke:black;stroke-width:0.75" />
                <polyline points="9.8,7 10.8,8 10.4,8.4 9.4,7.4"
                style="fill:none;stroke:black;stroke-width:0.75" />
                <polyline points="14.8,1.4 15.4,1.6 15.8,1.8 15.9,2.3"
                style="fill:none;stroke:black;stroke-width:0.75" />
                <polyline points="13.94,2.6 13.2,2.6 12.8,2.8 10,5.8"
                style="fill:none;stroke:black;stroke-width:0.75" />
            </svg>
        </button>

        <button class="papierkorbButton" @click="$emit('delete-todo-event', todo.id)">
            <svg class="papierkorb" height="20" width="20">
                <polyline points="5,7.5 5,15 7.5,17.5 12.5,17.5 15,15 15,7.5"
                style="fill:none;stroke:black;stroke-width:1.5" />
                <polyline points="5,5 15,5 16,5.5 16.5,6.125 16,7 15,7.5 5,7.5 4,7 3.5,6.125 4,5.5 5,5 15,5"
                style="fill:none;stroke:black;stroke-width:1.5" />
                <polyline points="7.5,5 8.5625,3.0625  10,2.5 11.4375,3.0625 12.5,5"
                style="fill:none;stroke:black;stroke-width:1.5" />
            </svg>
        </button>
        <hr class="ToDoTrennlinie" noshade>
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
            // markCompleted() {
            //     console.log(this.todo.completed)
            //     this.todo.completed = !this.todo.completed
            //     console.log(this.todo.completed)
            // },
            deleteToDoItem(toDoId){
                this.todoEntries = this.todoEntries.filter(item => item.id !== toDoId)

            },
            editToDoItem(toDoId){
				console.log(this.todoEntries[toDoId - 1].description)
			},
            showEditInput(toDoId){
                console.log("hallo")
                let id;
                console.log(this.todo_prop)
				for (const element of this.todo_prop) {
                    console.log(element)
					if(toDoId == element.id){
						id = element.id
                        console.log(element)

					}
				}
                var x = document.getElementsByClassName("editInput")[id];
                if (x.style.display === "none") {
                    x.style.display = "unset";
                } else {
                    x.style.display = "none";
                }
            }
        }
    }
    console.log("item end")
</script>

