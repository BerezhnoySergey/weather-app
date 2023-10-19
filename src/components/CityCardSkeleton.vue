<template>
	<div class="card">
		<div class="card-wrap">
			<AnimatedPlaceholder class="card-widht" />
			<AnimatedPlaceholder class="card-widht__min" />
		</div>

		<div class="placeholder">
			<AnimatedPlaceholder class="placeholder-width" />
			<AnimatedPlaceholder class="placeholder-width__min" />
		</div>
	</div>
</template>

<script setup lang="ts">
import AnimatedPlaceholder from "./AnimatedPlaceholder.vue";
</script>
<style scoped>
.card {
	display: flex;
	padding-top: 1.5rem;
	padding-bottom: 1.5rem;
	padding-left: 0.75rem;
	padding-right: 0.75rem;
	background-color: #1f2833;
	border-radius: 0.375rem;
}
.card-wrap {
	display: flex;
	flex: 1 1 0%;
	flex-direction: column;
	gap: 0.5rem;
}
.card-widht {
	max-width: 50%;
}
.card-widht__min {
	max-width: 40%;
}
.placeholder {
	display: flex;
	flex-direction: column;
	align-items: flex-end;
	flex: 1 1 0%;
	gap: 0.5rem;
}
.placeholder-width__min {
	max-width: 75px;
	width: 100%;
}
.placeholder-width {
	max-width: 50px;
	width: 100%;
}
</style>
