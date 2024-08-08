<template>
	<div class="hero-list">
		<h1>Marvel Heroes</h1>
		<div v-if="loading" class="loading-message">Loading...</div>
		<div v-else class="hero-container">
			<HeroCard
				v-for="hero in heroes"
				:key="hero.id"
				:hero="hero"
				@edit-hero="editHero(hero)"
				@delete-hero="deleteHero(hero.id)"
			/>
		</div>
		<HeroForm
			v-if="selectedHero"
			:hero="selectedHero"
			:editMode="editMode"
			@onSave="saveHero"
		/>
	</div>
</template>

<script>
import axios from "axios";
import CryptoJS from "crypto-js";
import HeroCard from "./HeroCard.vue";
import HeroForm from "./HeroForm.vue";

export default {
	name: "HeroList",
	components: {
		HeroCard,
		HeroForm,
	},
	data() {
		return {
			heroes: [],
			loading: true,
			selectedHero: null,
			editMode: false,
		};
	},
	created() {
		this.fetchHeroes();
	},
	methods: {
		async fetchHeroes() {
			const publicKey = "2de0b8c253469e492909b33b198e3444";
			const ts = new Date().getTime();
			const hash = this.generateHash(
				ts,
				"a47e9becc01f375c578475016278d841b7afabb7",
				publicKey
			);

			try {
				const response = await axios.get(
					"https://gateway.marvel.com/v1/public/characters",
					{
						params: {
							apikey: publicKey,
							ts,
							hash,
							limit: 15,
						},
					}
				);
				this.heroes = response.data.data.results;
			} catch (error) {
				console.error("Error fetching heroes:", error);
				this.heroes = [];
			} finally {
				this.loading = false;
			}
		},
		generateHash(ts, privateKey, publicKey) {
			return CryptoJS.MD5(ts + privateKey + publicKey).toString();
		},
		editHero(hero) {
			this.selectedHero = { ...hero };
			this.editMode = true;
		},
		deleteHero(id) {
			this.heroes = this.heroes.filter((hero) => hero.id !== id);
		},
		saveHero(hero) {
			if (this.editMode) {
				const index = this.heroes.findIndex((h) => h.id === hero.id);
				if (index !== -1) {
					this.heroes.splice(index, 1, hero);
				}
			} else {
				this.heroes.push({ ...hero, id: Date.now() });
			}
			this.selectedHero = null;
			this.editMode = false;
		},
	},
};
</script>

<style scoped>
h1 {
	margin-bottom: 40px; 
}

.hero-list {
	display: flex;
	flex-direction: column;
	align-items: center;
}

.loading-message {
	font-size: 1.5em;
	color: #fff;
	margin: 20px;
}

.hero-container {
	display: flex;
	flex-wrap: wrap;
	gap: 20px;
	justify-content: center;
	width: 100%;
}
</style>
