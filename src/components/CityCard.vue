<template>
	<div class="city">
		<div class="city-wrap">
			<h2 class="city-name">{{ city.city }}</h2>
			<h3 class="city-sub">{{ city.state }}</h3>
		</div>
		<div class="city-wrap__temp">
			<p class="city-currrent__temp city-sub">
				{{ Math.round(city.weather.current.temp) }}&deg;
			</p>
			<div class="city-daily">
				<span class="city-daily__text"
					>H: {{ Math.round(city.weather.daily[0].temp.max) }}&deg;</span
				>
				<span class="city-daily__text"
					>L: {{ Math.round(city.weather.daily[0].temp.min) }}&deg;</span
				>
			</div>
		</div>
	</div>
</template>

<script setup lang="ts">
defineProps({
	city: {
		type: Object,
		default: () => ({}),
	},
});
</script>

<style scoped>
.city {
	display: flex;
	padding-top: 1.5rem;
	padding-bottom: 1.5rem;
	background-color: #1f2833;
	border-radius: 0.375rem;
	padding-right: 20px;
	padding-left: 20px;
	transition: all 0.2s linear;
	cursor: pointer;
}
.city:hover {
	background-color: rgb(0, 57, 83);
}
.city-wrap {
	display: flex;
	flex-direction: column;
	flex: 1 1 0%;
}
.city-name {
	font-size: 1.875rem;
	line-height: 2.25rem;
	margin: 0;
}
.city-sub {
	margin: 0;
}
.city-wrap__temp {
	display: flex;
	flex-direction: column;
	gap: 0.5rem;
}
.city-currrent__temp {
	align-self: flex-end;
	font-size: 1.875rem;
	line-height: 2.25rem;
}
.city-daily {
	display: flex;
	gap: 0.5rem;
}
.city-daily__text {
	font-size: 0.75rem;
	line-height: 1rem;
}
</style>
