<template>
	<div id="app" class="app-container">
		<HeroList />
	</div>
</template>

<script>
import HeroList from "./components/HeroList.vue";

export default {
	name: "App",
	components: {
		HeroList,
	},
};
</script>

<style scoped>
.app-container {
	display: flex;
	flex-direction: column;
	align-items: center;
	padding: 20px;
	min-height: 100vh;
}
</style>
