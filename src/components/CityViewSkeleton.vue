<template>
	<div class="wrap">
		<!-- Overview -->
		<div class="overview">
			<AnimatedPlaceholder class="overview-placeholder" />
			<AnimatedPlaceholder class="overview-placeholder__two" />
			<AnimatedPlaceholder class="overview-placeholder__three" />
			<AnimatedPlaceholder class="overview-placeholder__four" />
			<AnimatedPlaceholder class="overview-placeholder__five" />
		</div>
		<!-- Hourly -->
		<div class="hourly">
			<AnimatedPlaceholder class="hourly-holder" />
		</div>
		<!-- Weekly -->
		<div class="hourly">
			<AnimatedPlaceholder class="hourly-holder" />
		</div>
	</div>
</template>

<script setup lang="ts">
import AnimatedPlaceholder from "./AnimatedPlaceholder.vue";
</script>
<style scoped>
.wrap {
	display: flex;
	flex-direction: column;
	flex: 1 1 0%;
}
.overview {
	display: flex;
	flex-direction: column;
	align-items: center;

	padding-top: 3rem;
	padding-bottom: 3rem;
}
.overview-placeholder {
	max-width: 300px;
	width: 100%;
	margin-bottom: 0.5rem;
}
.overview-placeholder__two {
	max-width: 300px;
	width: 100%;
	margin-bottom: 3rem;
}
.overview-placeholder__three {
	max-width: 300px;
	height: 100px;
	width: 100%;
	margin-bottom: 3rem;
}
.overview-placeholder__four {
	max-width: 300px;
	width: 100%;
	margin-bottom: 2rem;
}
.overview-placeholder__five {
	max-width: 300px;
	height: 75px;
	width: 100%;
}
.hourly {
	display: flex;
	flex-direction: column;
	align-items: center;
	padding-top: 3rem;
	padding-bottom: 3rem;
	padding-left: 2rem;
	padding-right: 2rem;
}
.hourly-holder {
	height: 100px;
	width: 100%;
	margin-bottom: 3rem;

	@media (min-width: 768px) {
		max-width: 768px;
	}
}
</style>
