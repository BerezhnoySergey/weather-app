<template>
	<div class="app-spiner">
		<div class="app-spiner__overlay"></div>
		<div class="app-spiner__icon"></div>
	</div>
</template>
<style scoped>
.app-spiner {
	position: absolute;
	top: 0;
	left: 0;
	z-index: 3;
	width: 100%;
	height: 100%;
}
.app-spiner:focus {
	outline: none;
}
.app-spiner__icon {
	position: absolute;
	top: 50%;
	left: 50%;
	z-index: 1;
	width: 40px;
	height: 40px;
	background-repeat: no-repeat;
	background-image: url("../img/Песочные\ часы.gif");
}
.app-spiner__overlay {
	width: 100%;
	height: 100%;
	background-color: rgba(white, 0.7);
}
</style>
