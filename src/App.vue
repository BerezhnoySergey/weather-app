<template>
	<div class="container-app">
		<SiteNavigation />
		<RouterView v-slot="{ Component }">
			<Transition name="page" mode="out-in">
				<component :is="Component" />
			</Transition>
		</RouterView>
	</div>
</template>

<script setup lang="ts">
import { RouterView } from "vue-router";
import SiteNavigation from "./components/SiteNavigation.vue";
</script>

<style scoped>
.container-app {
	display: flex;
	flex-direction: column;
	min-height: 100vh;
	font-family: Roboto, sans-serif;
	background-color: #1f2833eb;
}
.page-enter-active,
.page-leave-active {
	transition: 600ms ease all;
}
.page-enter-from,
.page-leave-to {
	opacity: 0;
}
</style>
