<template>
	<header class="header">
		<div class="header-inner">
			<div class="container">
				<nav class="header-wrap">
					<RouterLink :to="{ name: 'home' }" class="desc-mod">
						<div class="wrapper">
							<i class="desc"></i>
							<p class="desc">Weather Forecast</p>
						</div>
					</RouterLink>

					<div class="modall">
						<i
							class="modall-control fa-solid fa-circle-info"
							@click="toggleModal"
						></i>
						<i
							class="modall-control fa-solid fa-plus"
							@click="addCity"
							v-if="route.query.preview && savedCities.length < 5"
						></i>
					</div>

					<BaseModal :modalActive="modalActive" @close-modal="toggleModal">
						<div class="toggle-modal">
							<h1 class="toggle-modal__title">About:</h1>
							<p class="toggle-modal__sub">
								The Local Weather allows you to track the current and future
								weather of cities of your choosing.
							</p>
							<h2 class="toggle-modal__caption">How it works:</h2>
							<ol class="toggle-modal__list">
								<li>
									Search for your city by entering the name into the search bar.
								</li>
								<li>
									Select a city within the results, this will take you to the
									current weather for your selection.
								</li>
								<li>
									Track the city by clicking on the "+" icon in the top right.
									This will save the city to view at a later time on the home
									page.
								</li>
							</ol>

							<h2 class="toggle-modal__caption">Removing a city</h2>
							<p>
								If you no longer wish to track a city, simply select the city
								within the home page. At the bottom of the page, there will be
								am option to delete the city.
							</p>
						</div>
					</BaseModal>
				</nav>
			</div>
		</div>
	</header>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { uid } from "uid";
import { RouterLink, useRoute, useRouter } from "vue-router";
import BaseModal from "./BaseModal.vue";

const modalActive = ref<boolean>(false);
const savedCities = ref<any>([]);
const route = useRoute();
const router = useRouter();

const toggleModal = (): void => {
	modalActive.value = !modalActive.value;
};

const addCity = (): void => {
	if (localStorage.getItem("savedCities")) {
		savedCities.value = JSON.parse(
			localStorage.getItem("savedCities") as string
		);
	}
	const locationObj = {
		id: uid(),
		state: route.params.state,
		city: route.params.city,
		cords: {
			lat: route.query.lat,
			lon: route.query.lon,
		},
	};
	savedCities.value.push(locationObj);
	localStorage.setItem("savedCities", JSON.stringify(savedCities.value));

	let query = Object.assign({}, route.query);
	delete query.preview;
	query.id = locationObj.id;
	router.replace({ query });
};
</script>
<style scoped>
.header {
	position: sticky;
	top: 0px;
	background-color: #1f2833;
}
.header-inner {
	display: flex;
	justify-content: center;
}
.container {
	width: 85%;
}
.header-wrap {
	padding: "2rem";
	display: flex;
	align-items: center;
	gap: 1rem;
	color: #c5c6c7;
	padding-top: 1.5rem;
	padding-bottom: 1.5rem;
	height: 2.5rem;
}
.wrapper {
	display: flex;
	align-items: center;
	gap: 0.75rem;
}
.desc {
	text-decoration: none;
	font-size: 1.5rem;
	line-height: 2rem;
	transition-duration: 150ms;
	cursor: pointer;
	color: #c5c6c7;
}
.desc:hover {
	color: #45a29e;
}
.desc-mod {
	text-decoration: none;
}
.modall {
	display: flex;
	gap: 0.75rem;
	flex: 1 1 0%;
	justify-content: flex-end;
}
.modall-control {
	font-size: 1.25rem;
	line-height: 1.75rem;
	transition-duration: 150ms;
	cursor: pointer;
}
.modall-control:hover {
	color: #45a29e;
}
.toggle-modal {
	color: black;
}
.toggle-modal__title {
	font-size: 1.5rem;
	line-height: 2rem;
	margin-bottom: 0.25rem;
}
.toggle-modal__sub {
	margin-bottom: 1rem;
}
.toggle-modal__caption {
	font-size: 1.5rem;
	line-height: 2rem;
}
.toggle-modal__list {
	list-style-type: decimal;
	list-style-position: inside;
	margin-bottom: 1rem;
}
</style>
