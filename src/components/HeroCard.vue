<template>
	<div class="hero-card">
		<h2>{{ hero.name }}</h2>
		<img
			:src="hero.thumbnail.path + '.' + hero.thumbnail.extension"
			alt="Hero image"
		/>
		<p class="description">
			{{ hero.description || "No description available" }}
		</p>
		<div class="button-container">
			<button @click="editHero">Edit</button>
			<button @click="deleteHero">Delete</button>
		</div>
	</div>
</template>

<script>
export default {
	name: "HeroCard",
	props: {
		hero: Object,
	},
	methods: {
		editHero() {
			this.$emit("edit-hero", this.hero);
		},
		deleteHero() {
			this.$emit("delete-hero", this.hero.id);
		},
	},
};
</script>

<style scoped>
.hero-card {
	border: 1px solid #ddd;
	border-radius: 8px;
	padding: 16px;
	margin-bottom: 16px;
	text-align: center;
	width: 200px;
	background-color: #fff;
	color: #000;
	position: relative;
	min-height: 400px; 
}

.hero-card img {
	max-width: 100%;
	height: auto;
	width: 150px;
}

.description {
	overflow: hidden;
	text-overflow: ellipsis;
	white-space: nowrap;
	margin: 10px 0; 
}

.button-container {
	position: absolute;
	bottom: 16px; 
	left: 0;
	right: 0; 
	display: flex;
	justify-content: center; 
	gap: 10px;
}

.hero-card button {
	padding: 8px 16px; 
	border: none;
	border-radius: 4px;
	cursor: pointer;
	color: white; 
}

.hero-card button:first-of-type {
	background-color: #007bff; 
}

.hero-card button:first-of-type:hover {
	background-color: #0056b3; 
}

.hero-card button:last-of-type {
	background-color: #dc3545; 
}

.hero-card button:last-of-type:hover {
	background-color: #c82333; 
}
</style>
