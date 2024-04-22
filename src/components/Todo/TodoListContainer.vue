<template>
<div class="todo-list-container">
	<TodoList
		@tick-todo-item="tickTodoItem"
		@add-new-todo-item="addNewTodoItem"
		@on-drop-todo-item="onDropTodoItem"
		:data="data.todo"
		:list-type="LIST_TYPE.TODO" />
	<TodoList
		@tick-todo-item="tickTodoItem"
		@on-drop-todo-item="onDropTodoItem"
		:data="data.done"
		:list-type="LIST_TYPE.DONE" />
</div>
</template>

<script setup>
import LIST_TYPE from '@/constants/LIST_TYPE';
import TodoList from './TodoList.vue';
import { ref } from 'vue';

let globalId = ref(2);
const data = ref({
	todo: [
		{id: 1, value: "Wash dishes", checked: false}
	],
	done: [
		{id: 2, value: "Clean car", checked: true}
	]
});

console.log(data.value)

const tickTodoItem = (item, listType) => {
	if (LIST_TYPE.TODO == listType) {
		data.value.todo = data.value.todo.filter(currItem => item.id != currItem.id);
		data.value.done = [...data.value.done, {
			...item,
			checked: !item.checked
		}];
	}

	if (LIST_TYPE.DONE == listType) {
		data.value.done = data.value.done.filter(currItem => item.id != currItem.id);
		data.value.todo = [...data.value.todo, {
			...item,
			checked: !item.checked
		}];
	}
};

const onDropTodoItem = (event, listType) => {
	const itemId = event.dataTransfer.getData("itemId");
	const dragStartListType = event.dataTransfer.getData("dragStartListType");

	if (listType == dragStartListType)
		return;

	let item;

	// Ako dropujem u todo, znaci da sam podatke uzeo iz done liste
	if (LIST_TYPE.TODO == listType) {
		item = data.value.done.find(x => x.id == itemId)
	}
	// Ako dropujem u done, znaci da sam podatke uzeo iz todo liste
	if (LIST_TYPE.DONE == listType) {
		item = data.value.todo.find(x => x.id == itemId)
	}

	if (LIST_TYPE.TODO == listType) {
		data.value.done = data.value.done.filter(currItem => item.id != currItem.id);
		data.value.todo = [...data.value.todo, {
			...item,
			checked: !item.checked
		}];
	}

	if (LIST_TYPE.DONE == listType) {
		data.value.todo = data.value.todo.filter(currItem => item.id != currItem.id);
		data.value.done = [...data.value.done, {
			...item,
			checked: !item.checked
		}];
	}
}

const addNewTodoItem = () => {
	data.value.todo = [...data.value.todo, {
		id: ++globalId.value,
		value: 'Add new checkbox',
		checked: false
	}];
};
</script>

<style scoped>
.todo-list-container {
	display: grid;
	grid-template-columns: repeat(2, 1fr);
	gap: 2rem;
	position: relative;
	z-index: 10;
}
</style>