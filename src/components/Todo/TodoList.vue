<template>
<div class="todo-list">
	<p>To do</p>
	<hr class="separator">
</div>
</template>

<script setup>
import { defineProps } from 'vue';
const props = defineProps(['data']);

console.log(props.data)
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

.separator {
	border: 1px solid #ebebeb;
	margin: 1rem 0;
}

.checkmark-container {
  display: block;
  position: relative;
  padding-left: 30px;
  cursor: pointer;
}

.checkmark-container input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

.checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 20px;
  width: 20px;
  background-color: white;
  border-radius: 2px;
  border: 2px solid gray;
}

.checkmark-container input:checked ~ .checkmark {
  background-color: #425BD9;
  border: none;
}

.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}

.checkmark-container input:checked ~ .checkmark:after {
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