<template>
	<div class="city-wrap">
		<!--Banner-->
		<div v-if="route.query.preview" class="banner-text">
			<p>
				You are currently previewing this city, click the "+" icon to start
				tracking this city.
			</p>
		</div>
		<!--Weather Overview-->
		<div class="overview-wrap">
			<h1 class="overview-h1">{{ route.params.city }}</h1>
			<p class="overview-p">
				{{
					new Date(weatherData.currentTime).toLocaleDateString("en-us", {
						weekday: "short",
						day: "2-digit",
						month: "long",
					})
				}}
				{{
					new Date(weatherData.currentTime).toLocaleTimeString("en-us", {
						timeStyle: "short",
					})
				}}
			</p>
			<p class="overview-temp">
				{{ Math.round(weatherData.current.temp) }}&deg;
			</p>
			<p>
				Feels like
				{{ Math.round(weatherData.current.feels_like) }} &deg;
			</p>
			<p class="overview-description">
				{{ weatherData.current.weather[0].description }}
			</p>
			<img
				class="overview-img"
				:src="`http://openweathermap.org/img/wn/${weatherData.current.weather[0].icon}@2x.png`"
				alt=""
			/>
		</div>

		<hr class="overview-hr" />

		<!--Hourly Weather-->
		<div class="hourly-wrap">
			<div class="hourly-text">
				<h2 class="hourly-name">Hourly Weather</h2>
				<div class="hourly-block">
					<div
						v-for="hour in weatherData.hourly"
						:key="hour.dt"
						class="hourly-item"
					>
						<p class="hourly-time">
							{{
								new Date(hour.currentTime).toLocaleTimeString("en-us", {
									hour: "numeric",
								})
							}}
						</p>
						<img
							class="hourly-img"
							:src="`http://openweathermap.org/img/wn/${hour.weather[0].icon}@2x.png`"
							alt=""
						/>
						<p class="hourly-hour_temp">{{ Math.round(hour.temp) }}&deg;</p>
					</div>
				</div>
			</div>
		</div>

		<hr class="overview-hr" />

		<!--Weekly Weather-->
		<div class="weekly">
			<div class="weekly-wrap">
				<h2 class="weekly-name">7 Day Forecast</h2>
				<div v-for="day in weatherData.daily" :key="day.dt" class="weekly-day">
					<p class="weekly-day_name">
						{{
							new Date(day.dt * 1000).toLocaleDateString("en-us", {
								weekday: "long",
							})
						}}
					</p>
					<img
						class="weekly-img"
						:src="`http://openweathermap.org/img/wn/${day.weather[0].icon}@2x.png`"
						alt=""
					/>
					<div class="weekly-temp">
						<p>H: {{ Math.round(day.temp.max) }}&deg;</p>
						<p>L: {{ Math.round(day.temp.min) }}&deg;</p>
					</div>
				</div>
			</div>
		</div>

		<div class="remove-btn city-remove" @click="toggleModal">
			<i class="basket fa-solid fa-trash"></i>
			<p class="no-margin">Remove City</p>
		</div>
	</div>
	<BaseModal :modalActive="modalActive" @close-modal="toggleModal">
		<div class="modal-info">
			1.Confirm Deletion: If you are sure you want to delete the city, confirm
			your choice by clicking "Yes" or a similar button in the pop-up window.
		</div>
		<div class="modal-info">
			2.City Deleted: After confirming the deletion, the selected city will be
			immediately removed from your list of favorites. The modal window will
			close, and you will no longer see the deleted city in your favorites list.
		</div>
		<div class="modal-info">
			This way, clicking the delete button in the modal window allows you to
			confirm the deletion of the city, and it will be removed from your list of
			favorites once confirmed.
		</div>

		<button @click="removeCity" class="remove-btn">Confirm remove</button>
	</BaseModal>
</template>

<script setup lang="ts">
import axios from "axios";
import { ref } from "vue";
import { useRoute, useRouter } from "vue-router";
import BaseModal from "./BaseModal.vue";

const route = useRoute();
const router = useRouter();

const modalActive = ref<boolean>(false);

const toggleModal = (): void => {
	modalActive.value = !modalActive.value;
};

const getWeatherData = async (): Promise<any> => {
	try {
		const weatherData = await axios.get(
			`https://api.openweathermap.org/data/2.5/onecall?lat=${route.query.lat}&lon=${route.query.lon}&exclude={part}&appid=7efa332cf48aeb9d2d391a51027f1a71&units=metric`
		);
		// cal current date & time
		const localOffset = new Date().getTimezoneOffset() * 60000;
		const utc = weatherData.data.current.dt * 1000 + localOffset;
		weatherData.data.currentTime =
			utc + 1000 * weatherData.data.timezone_offset;

		// cal hourly weather offset
		weatherData.data.hourly.forEach((hour: any) => {
			const utc = hour.dt * 1000 + localOffset;
			hour.currentTime = utc + 1000 * weatherData.data.timezone_offset;
		});

		// Delay
		await new Promise((resolve) => setTimeout(resolve, 1000));

		return weatherData.data;
	} catch (error) {
		console.log(error);
	}
};

const weatherData = await getWeatherData();
const removeCity = (): void => {
	const cities = JSON.parse(localStorage.getItem("savedCities") as string);
	const updatedCities = cities.filter(
		(city: any) => city.id !== route.query.id
	);

	localStorage.setItem("savedCities", JSON.stringify(updatedCities));
	router.push({ name: "home" });
};
</script>

<style scoped>
.city-wrap {
	display: flex;
	flex-direction: column;
	align-items: center;
	flex: 1 1 0%;
}
.banner-text {
	padding: 1rem /* 16px */;
	width: 100%;
	text-align: center;
	background-color: #37475b99;
	color: white;
}
.modal-info {
	margin-bottom: 0.75rem;
}
.overview-wrap {
	display: flex;
	align-items: center;
	flex-direction: column;
	padding-top: 3rem;
	padding-bottom: 3rem;
	color: #fff;
}
.overview-h1 {
	font-size: 2.25rem;
	line-height: 2.5rem;
	margin-bottom: 0.5rem;
}
.overview-p {
	font-size: 0.875rem;
	line-height: 1.25rem;
	margin-bottom: 3rem;
}
.overview-temp {
	font-size: 6rem;
	line-height: 1;
	margin-bottom: 2rem;
}
.overview-description {
	text-transform: capitalize;
}
.overview-img {
	width: 150px;
	height: auto;
}
.overview-hr {
	border-color: white;
	opacity: 0.1;
	border-width: 1px;
	width: 100%;
}
.no-margin {
	margin: 0;
}
.hourly-wrap {
	width: 100%;
	padding-top: 3rem;
	padding-bottom: 3rem;
	@media (min-width: 768px) {
		max-width: 768px;
	}
}
.hourly-text {
	color: white;
}
.hourly-name {
	margin-bottom: 1rem;
	@media (max-width: 768px) {
		margin-left: 1.5rem;
	}
}
.hourly-block {
	display: flex;
	gap: 2.5rem;
	overflow-x: scroll;
}
.hourly-item {
	display: flex;
	flex-direction: column;
	gap: 1rem;
	align-items: center;
}
.hourly-time {
	white-space: nowrap;
}
.hourly-img {
	width: auto;
	height: 50px;
	object-fit: cover;
}
.hourly-hour_temp {
	font-size: 1.25rem;
	line-height: 1.75rem;
}
.weekly {
	width: 100%;
	padding-top: 3rem;
	padding-bottom: 3rem;

	@media (min-width: 768px) {
		max-width: 768px;
	}
}

.weekly-wrap {
	margin-left: 2rem;
	margin-right: 2rem;
	color: white;
}
.weekly-name {
	margin-bottom: 1rem;
}
.weekly-day {
	display: flex;
	align-items: center;
}
.weekly-day_name {
	flex: 1 1 0%;
}
.weekly-img {
	width: 50px;
	height: 50px;
	object-fit: cover;
}
::-webkit-scrollbar {
	width: 6px;
	height: 6px;
}
.weekly-temp {
	display: flex;
	gap: 0.5rem;
	flex: 1 1 0%;
	justify-content: flex-end;
}
.remove-btn {
	color: white;
	cursor: pointer;
	padding-top: 0.5rem;
	padding-bottom: 0.5rem;
	padding-left: 1.5rem;
	padding-right: 1.5rem;
	background-color: #1f2833;
	border-radius: 0.5rem;
	transition-duration: 500ms;
	margin-bottom: 1.5rem;
	border: none;
}
.basket {
	margin-right: 0.5rem;
}
.remove-btn:hover {
	background-color: #45a29e;
}
.city-remove {
	display: flex;
	align-items: center;
}
::-webkit-scrollbar-thumb {
	border-radius: 3px;
}

::-webkit-scrollbar-thumb:hover {
	background-color: #888;
}

::-webkit-scrollbar-track {
	background-color: transparent;
	border-radius: 3px;
}
</style>
