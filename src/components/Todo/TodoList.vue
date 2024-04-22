<template>
<div class="todo-list">
	<div
	class="todo-list-header">
		<p v-if="props.listType == LIST_TYPE.DONE">Done</p>
		<p v-else>To do</p>

		<p v-if="props.listType == LIST_TYPE.DONE">Delete</p>
		<p
			v-else
			@click="$emit('addNewTodoItem')">Add</p>
	</div>

	<hr class="separator">

	<div
		class="drop-zone"
		@drop="$emit('onDropTodoItem', $event, props.listType)"
		@dragenter.prevent @dragover.prevent>
		<div class="item-container">
			<div class="todo-item"
				@dragstart="startDrag($event, item)"
				draggable="true"
				:class="'todo-item-' + item.id"
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
					@focus="addDarkBackground(item.id)"
					@blur="removeDarkBackground(item.id)"
					v-model="item.value" />
			</div>
		</div>
	</div>
</div>
</template>

<script setup>
import { defineProps } from 'vue';
import LIST_TYPE from '@/constants/LIST_TYPE';
const props = defineProps(['data', 'listType']);

// Posto container citav treba biti tamniji na fokusu, morao sam uraditi ovaj work-around
// kako bi se prikazao tamni background na cijelom parent containeru, a ne samo na inputu

const addDarkBackground = (id) => {
	document.querySelector(`.todo-item-${id}`).classList.add('dark-input');
}
const removeDarkBackground = (id) => {
	document.querySelector(`.todo-item-${id}`).classList.remove('dark-input');
}

const startDrag = (event, item) => {
	console.log("on start drag")
	console.log(item)
	event.dataTransfer.dropEffect = "move"
	event.dataTransfer.effectAllowed = "move"
	event.dataTransfer.setData("itemId", item.id)
	event.dataTransfer.setData("dragStartListType", props.listType)
}

</script>

<style scoped>
.drop-zone {
	height: 100%;
}

.dark-input {
	background: rgba(34, 34, 34, 0.1);
}

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
	gap: 1rem;
}

.todo-item {
	display: flex;
	align-items: center;
	gap: .75rem;
	border-radius: 4px;
	padding: .5rem;
	padding-right: 0;
}

.todo-item input[type=text] {
	border: none;
	outline: none;
	font-family: "Roboto";
	color: #222222;
	font-size: 14px;
	width: 100%;
	background: none;
}

.todo-item input[type=text]:disabled {
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