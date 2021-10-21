<template>
	<form @submit.prevent="handleEdit">
		<label>Title:</label>
		<input type="text" v-model="title" required />
		<label>Details:</label>
		<textarea v-model="details" required></textarea>
		<button>Add project</button>
	</form>
</template>

<script>
export default {
	props: ["id"],
	data() {
		return {
			title: "",
			details: "",
			uri: "http://localhost:3000/projects/" + this.id,
		};
	},
	mounted() {
		fetch(this.uri)
			.then((res) => res.json())
			.then((data) => {
				console.log(data);
				this.title = data.title;
				this.details = data.details;
			});
	},
	methods: {
		handleEdit(id) {
			let updateProject = {
				title: this.title,
				details: this.details,
			};
			fetch(this.uri, {
				method: "PATCH",
				headers: { "Content-Type": "application/json" },
				body: JSON.stringify(updateProject),
			})
				.then(() => {
					this.$router.push("/");
				})
				.catch((err) => console.log(err.message));
		},
	},
};
</script>

<style></style>
