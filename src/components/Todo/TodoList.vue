<template>
<div class="todo-list">
	<div
	class="todo-list-header">
		<p v-if="props.listType == LIST_TYPE.DONE">Done</p>
		<p v-else>To do</p>

		<img
			v-if="props.listType == LIST_TYPE.DONE"
			class="todo-list-delete-icon"
			src="@/assets/icons/DeleteIcon.svg"
			@click="$emit('deleteAllDoneItems')"
			/>
		<p
			v-else
			class="todo-list-add-icon"
			@click="$emit('addTodoItem')">+</p>
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
					v-model="item.value" />

				<button
					@click="showTodoItemOptions($event, item)"
					class="todo-item-options-button">
					<img src="@/assets/icons/EllipsisIcon.png" />
				</button>
				
				<div v-if="todoItemOptionStates[item.id]" class="todo-item-options">
					<div
						@click="$emit('deleteTodoItem', item.id, props.listType)"
						class="todo-item-option">
						<img
							class="todo-list-delete-icon"
							src="@/assets/icons/DeleteIcon.svg"/>
						<p>Delete</p>
					</div>
				</div>
			</div>
		</div>
	</div>
</div>
</template>

<script setup>
import { ref, defineProps } from 'vue';
import LIST_TYPE from '@/constants/LIST_TYPE';
const props = defineProps(['data', 'listType']);

const todoItemOptionStates = ref({});

const showTodoItemOptions = (event, item) => {
	console.log(event.target)
	if (event.target )

	if (todoItemOptionStates.value[item.id] == undefined) {
		todoItemOptionStates.value = {};
		todoItemOptionStates.value[item.id] = true;
	}
	else
		todoItemOptionStates.value[item.id] = !todoItemOptionStates.value[item.id];
}

const startDrag = (event, item) => {
	todoItemOptionStates.value = {};

	event.dataTransfer.dropEffect = "move";
	event.dataTransfer.effectAllowed = "move";
	event.dataTransfer.setData("itemId", item.id);
	event.dataTransfer.setData("dragStartListType", props.listType);
}

</script>

<style scoped>
.drop-zone {
	height: 100%;
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
	margin-bottom: 2rem;
}

.todo-list-add-icon, .todo-list-delete-icon {
	width: 20px;
	height: 20px;
	user-select: none;
}

.todo-list-add-icon {
	color: white;
	background: #7a7a7a;
	border-radius: 4px;
	display: flex;
	text-align: center;
	display: flex;
	align-items: center;
	justify-content: center;
	font-size: 18px;
}

.todo-list-delete-icon:hover {
	cursor: pointer;
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
	position: relative;
	gap: .75rem;
	border-radius: 4px;
	padding: .5rem;
	padding-right: 0;
}

.todo-item:hover {
	background: rgba(34, 34, 34, 0.1);
}

.todo-item:hover .todo-item-options-button {
	display: block;
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

.todo-item-options-button {
	position: absolute;
	right: 10px;
	top: 10px;
	display: none;
	border: none;
	outline: none;
	background: #e8e8e8;
}

.todo-item-options-button:hover {
	cursor: pointer;
}

.todo-item-options-button img {
	width: 16px;
	height: 16px;
}

.todo-item-options {
	display: block;
	position: absolute;
	transform: translate(100%);
	right: 0;
	border-radius: 4px;
	box-shadow: rgb(38, 57, 77) 0px 20px 30px -10px;
	background: white;
	min-width: 200px;
	z-index: 10;
}

.todo-item-option {
	display: flex;
	border-radius: 4px;
	gap: 1rem;
	padding: 1rem;
}

.todo-item-option p {
	font-weight: 400;
}

.todo-item-option:hover {
	background: #e8e8e8;
	cursor: pointer;
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