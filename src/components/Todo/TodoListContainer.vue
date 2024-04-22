<template>
<div class="todo-list-container">
	<TodoList :data="data.todo" />
	<TodoList :data="data.done"/>
</div>
</template>

<script setup>
import TodoList from './TodoList.vue';
import { ref } from 'vue';

const dataSchema = {
	todo: [
		{value: "Wash dishes", checked: false}
	],
	done: [
		{value: "Clean car", checked: true}
	]
};

const data = ref(dataSchema);
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