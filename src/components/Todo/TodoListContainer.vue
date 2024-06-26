<template>
<div class="todo-list-container">
	<TodoList
		@tick-todo-item="tickTodoItem"
		@add-todo-item="addTodoItem"
		@delete-todo-item="deleteTodoItem"
		@on-drop-todo-item="onDropTodoItem"
		:data="data.todo"
		:list-type="LIST_TYPE.TODO" />
	<TodoList
		@tick-todo-item="tickTodoItem"
		@delete-todo-item="deleteTodoItem"
		@delete-all-done-items="deleteAllDoneItems"
		@on-drop-todo-item="onDropTodoItem"
		:data="data.done"
		:list-type="LIST_TYPE.DONE" />
</div>
</template>

<script setup>
import LIST_TYPE from '@/constants/LIST_TYPE';
import TodoList from './TodoList.vue';
import { ref } from 'vue';

let globalId = ref(0);
const data = ref({
	todo: [],
	done: []
});

const addTodoItem = () => {
	data.value.todo = [...data.value.todo, {
		id: ++globalId.value,
		value: 'Add new checkbox',
		checked: false
	}];
};

const deleteTodoItem = (id, listType) => {
	data.value[listType] = data.value[listType].filter(currItem => id != currItem.id);
}

const deleteAllDoneItems = () => {
	data.value.done = []
}

const tickTodoItem = (item, listType) => {
	data.value[listType] = data.value[listType].filter(currItem => item.id != currItem.id);
	
	if (LIST_TYPE.TODO == listType) {
		data.value.done = [...data.value.done, {
			...item,
			checked: !item.checked
		}];
	}

	if (LIST_TYPE.DONE == listType) {
		data.value.todo = [...data.value.todo, {
			...item,
			checked: !item.checked
		}];
	}
};

const onDropTodoItem = (event, listType) => {
	const itemId = event.dataTransfer.getData("itemId");
	const dragStartListType = event.dataTransfer.getData("dragStartListType");

	// Prevent error where drag and drop payloads are from the same data point
	if (listType == dragStartListType)
		return;

	// If it gets dropped into todo, it means the dragged data is from the done list
	const item = data.value[listType == LIST_TYPE.TODO ? LIST_TYPE.DONE : LIST_TYPE.TODO].find(x => x.id == itemId)

	// Then update accordingly (remove from dragged list, transfer to dropped list)
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
</script>

<style scoped>
.todo-list-container {
	display: grid;
	grid-template-columns: repeat(2, 1fr);
	gap: 2rem;
	position: relative;
	z-index: 10;
}

@media (max-width: 768px) {
	.todo-list-container {
		grid-template-columns: 1fr;
	}
}
</style>