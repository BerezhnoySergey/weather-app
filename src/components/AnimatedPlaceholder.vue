<template>
	<div class="placeholder">&nbsp;</div>
</template>

<style scoped>
.placeholder {
	background-image: linear-gradient(to right, var(--tw-gradient-stops));
	--tw-gradient-from: #f3f4f6 var(--tw-gradient-from-position);
	--tw-gradient-to: rgb(243 244 246 / 0) var(--tw-gradient-to-position);
	--tw-gradient-stops: var(--tw-gradient-from), var(--tw-gradient-to);
	animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;

	@keyframes pulse {
		0%,
		100% {
			opacity: 1;
		}
		50% {
			opacity: 0.5;
		}
	}
}
</style>
