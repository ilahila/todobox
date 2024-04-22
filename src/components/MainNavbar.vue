<template>
<div class="outer-container">
	<div class="container">
		<div class="navbar-left">
			<img src="@/assets/icons/TodoboxLogo.svg" alt="Todobox logo" />
			<p>Todobox</p>
		</div>
		<div class="navbar-right">
			<p>Username</p>
			<img src="@/assets/images/UserImage.png" alt="User profile picture" />
		</div>
	</div>
</div>
</template>

<script setup>
</script>

<style scoped>
.outer-container {
	background: #232C33;
	padding: 1rem 0;
	color: white;
	position: relative;
	z-index: 1;
}

.container {
	display: flex;
	justify-content: space-between;
	align-items: center;
}

.navbar-left, .navbar-right {
	display: flex;
	align-items: center;
	gap: 1rem;
}

.navbar-right img {
	width: 40px;
	height: 40px;
	object-fit: cover;
	border-radius: 50%;
}

.navbar-left p {
	font-weight: bold;
}

.navbar-right p {
	font-size: .875rem;
}

@media (max-width: 600px) {
	.container {
		flex-direction: column;
		font-size: 24px;
		gap: 1.5rem;
	}
}
</style>