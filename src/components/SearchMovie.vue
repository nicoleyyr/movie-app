<template>
	<v-container v-if="loading">
		<div class="text-xs-center">
			<v-progress-circular indeterminate :size="150" :width="8" color="dark"></v-progress-circular>
		</div>
	</v-container>
	<v-container v-else grid-list-xl>
		<v-layout wrap>
			<v-flex xs4 v-for="(item, index) in searchresults" :key="index" mb-2>
				<v-card>
					<v-img v-if="item.poster_path != null" :src="imgURL + item.poster_path" aspect-ratio="1"></v-img>
					<v-img v-else src aspect-ratio="1">
						<div style="height: 370px" class="d-flex align-center justify-center">
							<p class="font-italic font-weight-medium">No Image</p>
						</div>
					</v-img>
					<v-card-title primary-title>
						<div>
							<h2>{{item.title}}</h2>
							<div>Date: {{item.release_date}}</div>
						</div>
					</v-card-title>
					<v-card-actions class="justify-center">
						<v-btn text color="green" @click="singleMovie(item.id)">View</v-btn>
					</v-card-actions>
				</v-card>
			</v-flex>
		</v-layout>
	</v-container>
</template>

<script>
import moment from "moment";
import { mapState } from "vuex";
export default {
	props: ["title"],
	data() {
		return {
			imgURL: "https://image.tmdb.org/t/p/w342"
		};
	},
	computed: {
		...mapState(["searchresults", "loading"])
	},
	created() {
		this.$store.dispatch("loadResults", this.title);
		this.$store.dispatch("loadingState");
	},
	methods: {
		singleMovie(id) {
			this.$router.push("/movie/" + id);
		},
		moment(date) {
			return moment(date).format("MMM Do YYYY");
		}
	},
	watch: {
		title(value) {
			this.$store.dispatch("loadResults", value);
		}
	}
};
</script>

<style lang="scss" scoped>
.v-progress-circular {
	margin: 1rem;
}
</style>
