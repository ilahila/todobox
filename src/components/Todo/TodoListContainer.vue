<template>
<div class="todo-list-container">
	<TodoList
		@tick-todo-item="tickTodoItem"
		@add-new-todo-item="addNewTodoItem"
		:data="data.todo"
		:list-type="LIST_TYPE.TODO" />
	<TodoList
		@tick-todo-item="tickTodoItem"
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