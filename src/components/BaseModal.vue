<template>
	<Teleport to="body">
		<Transition name="modal-outer">
			<div v-show="modalActive" class="modal">
				<Transition name="modal-inner">
					<div v-if="modalActive" class="modal-wrap">
						<slot></slot>
						<button @click="$emit('close-modal')" class="modal-btn">
							Close
						</button>
					</div>
				</Transition>
			</div>
		</Transition>
	</Teleport>
</template>

<script setup lang="ts">
defineEmits(["close-modal"]);

defineProps({
	modalActive: {
		type: Boolean,
		default: false,
	},
});
</script>

<style scoped>
.modal-outer-enter-active,
.modal-outer-leave-active {
	transition: opacity 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02);
}

.modal-outer-enter-from,
.modal-outer-leave-to {
	opacity: 0;
}

.modal-inner-enter-active {
	transition: all 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02) 0.15s;
}

.modal-inner-leave-active {
	transition: all 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02);
}

.modal-inner-enter-from {
	opacity: 0;
	transform: scale(0.8);
}

.modal-inner-leave-to {
	transform: scale(0.8);
}
.modal {
	position: absolute;
	width: 100%;
	position: fixed;
	height: 100vh;
	top: 0px;
	left: 0px;
	display: flex;
	justify-content: center;
	padding-left: 2rem;
	padding-right: 2rem;
	font-family: Roboto, sans-serif;
	@media (max-width: 480px) {
		width: 85%;
	}
}
.modal-wrap {
	padding: 1rem;
	align-self: flex-start;
	margin-top: 8rem;
	background: #c5c6c7;
	border-radius: 0.75rem;
	font-weight: bold;
	@media (min-width: 768px) {
		max-width: 768px;
	}
}
.modal-btn {
	color: white;
	margin-top: 2rem;
	background-color: #1f2833;
	padding-top: 0.5rem;
	padding-bottom: 0.5rem;
	padding-left: 1.5rem;
	padding-right: 1.5rem;
	transition: 600ms ease all;
	border-radius: 0.5rem;
	margin-left: 1rem;
	cursor: pointer;
	border: none;
}
.modal-btn:hover {
	background: #45a29e;
}
</style>
