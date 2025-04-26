<template>
	<div class="input-container">
		<div class="index-view">
			<div class="col-view">col: {{ col }}</div>
			<div class="row-view">row: {{ row }}</div>
		</div>
	</div>
</template>

<script>
export default {
	name: "Inputs",
	props: {
		dataShot: Array,
		col: Number,
		row: Number,
	},
	data() {
		return {}
	},
	methods: {},
}
</script>

<style scoped>
.input-container {
	width: 100%;
	height: 100%;
	background-color: aqua;
}

.index-view {
	width: 8%;
	height: 100%;
	border-width: 1px;
	border-style: solid; /* Added border style */
	border-color: black; /* Added border color */
}
</style>
