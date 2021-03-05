<template>
	<div class="home">
		<v-row justify="center" align="center" height="100%" no-gutters>
			<v-text-field
				@keyup.enter="storeTodo"
				@click:append="storeTodo"
				v-model="newTodo"
				max-width="200"
				append-icon="mdi-send"
				label="Enter a todo"
			/>
		</v-row>
		<v-row justify="center" align="center" height="100%" no-gutters>
			<v-data-table
				:headers="headers"
				:items="toDoList"
				:items-per-page="5"
				class="elevation-1"
			>
				<template v-slot:item.completed="{ item }">
					<v-icon
						small
						:color="item.completed ? 'red' : 'green'"
						class="mr-2"
						@click="completeTodo(item)"
					>
						{{ item.completed ? "mdi-close" : "mdi-check" }}
					</v-icon>
				</template>
				<template v-slot:item.delete="{ item }">
					<v-icon small @click="removeTodo(item)">
						mdi-delete
					</v-icon>
				</template>
				<template v-slot:no-data>
					<v-btn color="primary">
						Reset
					</v-btn>
				</template>
			</v-data-table>
		</v-row>
	</div>
</template>

<script>
// @ is an alias to /src

export default {
	name: "Home",
	components: {},
	data() {
		return {
			headers: [
				{ text: "Todo's", value: "todo" },
				{ text: "Completed", value: "completed" },
				{ text: "Delete", value: "delete" },
			],
			toDoList: [],
			newTodo: "",
		};
	},
	mounted() {
		if (localStorage.getItem("todoList")) {
			this.toDoList = JSON.parse(localStorage.getItem("todoList"));
		}
	},
	methods: {
		storeTodo() {
			if (this.newTodo !== "")
				this.toDoList.push({ todo: this.newTodo, completed: false });
			localStorage.setItem("todoList", JSON.stringify(this.toDoList));
			this.newTodo = "";
		},
		removeTodo(i) {
			let index = this.toDoList.indexOf(i);
			if (index != -1) {
				this.toDoList.splice(index, 1);
				localStorage.setItem("todoList", JSON.stringify(this.toDoList));
			}
		},
		completeTodo(i) {
			let index = this.toDoList.indexOf(i);
			if (index != -1) {
				if (i.completed) {
					this.toDoList[index].completed = false;
				} else {
					this.toDoList[index].completed = true;
					localStorage.setItem("todoList", JSON.stringify(this.toDoList));
				}
			}
		},
	},
};
</script>
