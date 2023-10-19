<template>
	<p class="input-home_result" v-if="savedCities.length === 5">
		The limit of favorites is exceeded.
	</p>
	<div v-for="city in savedCities" :key="city.id">
		<CityCard :city="city" @click="goToCityView(city)" />
	</div>

	<p v-if="!savedCities.length">
		No locations added. To start tracking a location, search in the field above.
	</p>
</template>

<script setup lang="ts">
import { ref } from "vue";
import axios from "axios";
import CityCard from "./CityCard.vue";
import { useRouter } from "vue-router";
const savedCities = ref<any>([]);
const router = useRouter();

const getCities = async (): Promise<void> => {
	if (localStorage.getItem("savedCities")) {
		savedCities.value = JSON.parse(
			localStorage.getItem("savedCities") as string
		);

		const requests = <any>[];

		savedCities.value.forEach((city: any) => {
			requests.push(
				axios.get(
					`https://api.openweathermap.org/data/2.5/onecall?lat=${city.cords.lat}&lon=${city.cords.lon}&exclude={part}&appid=7efa332cf48aeb9d2d391a51027f1a71&units=metric`
				)
			);
		});

		const weatherData = await Promise.all(requests);

		//Delay

		await new Promise((resolve) => setTimeout(resolve, 1000));

		weatherData.forEach((value: any, index: number) => {
			savedCities.value[index].weather = value.data;
		});
	}
};

await getCities();

const goToCityView = (city: any): void => {
	router.push({
		name: "cityView",
		params: { state: city.state, city: city.city },
		query: { id: city.id, lat: city.cords.lat, lon: city.cords.lon },
	});
};
</script>
