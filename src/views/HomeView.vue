<template>
	<div class="home-inner">
		<main class="container">
			<div class="container-wrap">
				<input
					type="text"
					@input="getSearchResults()"
					v-model="searchQuery"
					placeholder="Search for a city or state"
					class="input-home"
				/>
				<ul class="input-home_ul" v-if="searchResults">
					<p v-if="searchError">Something went wrong. Please try again.</p>
					<p
						class="input-home_result"
						v-if="!searchError && searchResults.length === 0"
					>
						No results match your query, try a different term.
					</p>
					<template v-else>
						<li
							v-for="result in searchResults"
							:key="result.name + result.country"
							class="input-home_result"
							@click="previewCity(result)"
						>
							{{ `${result.name}, ${result.state}, ${result.country}` }}
						</li>
					</template>
				</ul>
			</div>
			<div class="home-wrap">
				<Suspense>
					<CityList />
					<template #fallback>
						<CityCardSkeleton />
					</template>
				</Suspense>
			</div>
		</main>
	</div>
</template>

<script setup lang="ts">
import CityCardSkeleton from "@/components/CityCardSkeleton.vue";
import CityList from "@/components/CityList.vue";
import axios from "axios";
import { ref } from "vue";
import { useRouter } from "vue-router";
import PreLoader from "../components/PreLoader.vue";

const router = useRouter();
const searchQuery = ref<string>("");
const queryTimeout = ref<number>(0);
const apiKey = "d204ad72789c0be0b3dd8d094cfa8ef8";
const searchResults = ref<any>(null);
const searchError = ref<any>(null);

const getSearchResults = (): void => {
	clearTimeout(queryTimeout.value);
	queryTimeout.value = setTimeout(async () => {
		if (searchQuery.value !== "") {
			try {
				const result = await axios.get(
					`http://api.openweathermap.org/geo/1.0/direct?q=${searchQuery.value}&limit=5&appid=${apiKey}`
				);
				searchResults.value = result.data;
			} catch (error) {
				searchError.value = true;
				searchResults.value = [];
			}
			return;
		}
		searchResults.value = null;
	}, 300);
};

const previewCity = ({
	name,
	state,
	lat,
	lon,
}: {
	name: string;
	state: string;
	lat: number;
	lon: number;
}): void => {
	router.push({
		name: "cityView",
		params: { city: name, state },
		query: { lat, lon, preview: "true" },
	});
};
</script>
<style scoped>
.container {
	color: #c5c6c7;
	margin-left: 0.5rem;
	margin-right: 0.5rem;
	width: 85%;
}
.home-inner {
	display: flex;
	justify-content: center;
}
.container-wrap {
	position: relative;
	padding-top: 1rem;
	margin-bottom: 2rem;
}
.input-home_ul {
	position: absolute;
	padding-left: 0.25rem;
	padding-right: 0.25rem;
	padding-top: 0.5rem;
	padding-bottom: 0.5rem;
	width: 100%;
	color: #c5c6c7;
	top: 2rem;
	background-color: #1f2833;
	list-style: none;
}
.input-home_result {
	padding-top: 0.5rem;
	padding-bottom: 0.5rem;
	cursor: pointer;
}
.input-home_result:hover {
	background-color: #455870;
}
.input-home {
	padding-left: 0.25rem;
	padding-right: 0.25rem;
	padding-top: 0.5rem;
	padding-bottom: 0.5rem;
	font-size: large;
	border-bottom-width: 1px;
	width: 100%;
	background-color: transparent;
	outline: none;
	min-width: 100%;
	border: none;
	border-bottom: 1px solid #c5c6c7;
	color: #c5c6c7;
}
.input-home:active,
:hover,
:focus {
	outline: 0;
	outline-offset: 0;
}
.home-wrap {
	display: flex;
	flex-direction: column;
	gap: 1rem;
}
</style>
