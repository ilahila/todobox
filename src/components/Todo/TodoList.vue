<template>
<div class="todo-list">
	<div class="todo-list-header">
		<p v-if="props.listType == LIST_TYPE.DONE">Done</p>
		<p v-else>To do</p>

		<p v-if="props.listType == LIST_TYPE.DONE">Delete</p>
		<p
			v-else
			@click="$emit('addNewTodoItem')">Add</p>
	</div>

	<hr class="separator">

	<div class="item-container">
		<div
			class="data-container"
			v-for="item in data"
			:key="item.id">
			<label class="checkmark-container">
				<input
					type="checkbox"
					:checked="item.checked"
					@click="$emit('tickTodoItem', item, props.listType)">
				<span class="checkmark"></span>
			</label>
			<input
				type="text"
				:disabled="item.checked"
				v-model="item.value" />
		</div>
	</div>
</div>
</template>

<script setup>
import { defineProps } from 'vue';
import LIST_TYPE from '@/constants/LIST_TYPE';
const props = defineProps(['data', 'listType']);
</script>

<style scoped>
.todo-list {
	background: white;
	padding: 1.5rem;
	border-radius: 5px;
	border: 1px solid #ebebeb;
	display: flex;
	flex-direction: column;
	gap: .5rem;
	min-height: 400px;
}

.todo-list p {
	font-weight: 500;
}

.todo-list-header {
	display: flex;
	justify-content: space-between;
}

.todo-list-header > p {
	cursor: pointer;
}

.separator {
	border: 1px solid #ebebeb;
	margin: 1rem 0;
}

.item-container {
	display: flex;
	flex-direction: column;
	gap: 2rem;
}

.data-container {
	display: flex;
	align-items: center;
	gap: .75rem;
}

.data-container input[type=text] {
	border: none;
	outline: none;
	font-family: "Roboto";
	color: #222222;
	font-size: 14px;
	width: 100%;
}

.data-container input[type=text]:disabled {
	background: none;
}

.checkmark-container {
  position: relative;
  cursor: pointer;
  font-size: 14px;
  display: flex;
  align-items: center;
}

.checkmark-container input[type=checkbox] {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

.checkmark {
  height: 20px;
  width: 20px;
  background-color: white;
  border-radius: 2px;
  border: 2px solid gray;
}

.checkmark-container input[type=checkbox]:checked ~ .checkmark {
  background-color: #425BD9;
  border: none;
}

.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}

.checkmark-container input[type=checkbox]:checked ~ .checkmark:after {
  display: block;
}

.checkmark-container .checkmark:after {
  left: 7px;
  top: 2px;
  width: 5px;
  height: 10px;
  border: solid white;
  border-width: 0 2px 2px 0;
  -webkit-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  transform: rotate(45deg);
}
</style>