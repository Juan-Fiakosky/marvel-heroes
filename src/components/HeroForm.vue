<template>
	<div class="hero-form">
		<h2 v-if="editMode">Edit Hero</h2>
		<h2 v-else>Add New Hero</h2>
		<form @submit.prevent="handleSubmit">
			<label for="name">Name:</label>
			<input type="text" v-model="localHero.name" id="name" />

			<label for="description">Description:</label>
			<textarea v-model="localHero.description" id="description"></textarea>

			<button type="submit">{{ editMode ? "Update" : "Add" }} Hero</button>
		</form>
	</div>
</template>

<script>
export default {
	name: "HeroForm",
	props: {
		hero: Object,
		editMode: Boolean,
	},
	data() {
		return {
			localHero: { ...this.hero },
		};
	},
	watch: {
		hero(newVal) {
			this.localHero = { ...newVal };
		},
	},
	methods: {
		handleSubmit() {
			this.$emit("onSave", this.localHero);
		},
	},
};
</script>

<style scoped>
.hero-form {
	margin: 20px;
	padding: 20px;
	border: 1px solid #ddd;
	border-radius: 8px;
	max-width: 400px;
	text-align: center;
	background-color: #32cd32;
	color: #000;
}

.hero-form form {
	display: flex;
	flex-direction: column;
}

.hero-form label {
	margin-top: 10px;
}

.hero-form input,
.hero-form textarea {
	padding: 8px;
	margin-top: 5px;
	border: 1px solid #ccc;
	border-radius: 4px;
}

.hero-form button {
	margin-top: 20px;
	padding: 10px;
	background-color: #007bff;
	color: white;
	border: none;
	border-radius: 4px;
	cursor: pointer;
}

.hero-form button:hover {
	background-color: #0056b3;
}
</style>
