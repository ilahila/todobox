<template>
<div class="todo-container">
	<div class="todo-description">
		<h1>Manage your todo list</h1>
		<p>Click on checkbox or drag and drop to done</p>
	</div>
	<div class="todo-illustration">
		<img src="@/assets/images/TodoHeaderIllustration.svg" alt="Todo app illustration" />
	</div>
</div>
</template>

<style scoped>
.todo-container {
	display: grid;
	grid-template-columns: 1fr 1fr;
	width: 100%;
	position: relative;
}

.todo-description {
	display: flex;
	flex-direction: column;
	padding: 5rem 0;
	gap: .5rem;
	position: relative;
	z-index: 10;
}

.todo-description > h1 {
	font-size: 3rem;
}

.todo-illustration {
	position: absolute;
	right: 0;
	top: -20%;
	z-index: 0;
}

.todo-illustration img {
	width: 500px;
}

@media (max-width: 768px) {
	.todo-illustration {
		right: -60%;
		top: 0;
	}
}
</style>