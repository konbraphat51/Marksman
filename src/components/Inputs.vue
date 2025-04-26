<template>
	<div class="input-container">
		<div class="index-view">
			<div class="col-view">col: {{ col + 1 }}</div>
			<div class="row-view">row: {{ row + 1 }}</div>
		</div>
		<div class="score-input">
			<div class="score-input-label">Score:</div>
			<input
				type="number"
				:value="score"
				@input="$emit('update:scoreX10', $event.target.value * 10)"
				min="0"
				max="10.9"
				step="0.1"
			/>
		</div>
		<div class="coordinates-view">
			<div class="coordinates-value">X: {{ x.toFixed(3) }}</div>
			<div class="coordinates-value">Y: {{ y.toFixed(3) }}</div>
		</div>
	</div>
</template>

<script>
export default {
	name: "Inputs",
	props: {
		col: Number,
		row: Number,
		x: Number,
		y: Number,
		scoreX10: Number,
	},
	emits: ["update:scoreX10"],
	data() {
		return {}
	},
	methods: {},
	computed: {
		score() {
			return (this.scoreX10 / 10).toFixed(1)
		},
	},
}
</script>

<style scoped>
.input-container {
	display: flex;
	flex-direction: row;
	width: 100%;
	height: 100%;
	background-color: aqua;
}

.index-view {
	width: auto;
	height: 100%;
	border-width: 1px;
	border-style: solid;
	border-color: black;
	padding-right: 5px;
	padding-left: 2px;
}

.score-input {
	display: flex;
	flex-direction: row;
	align-items: center;
	width: auto;
	height: 100%;
	border-style: solid;
	border-color: black;
	border-width: 1px;
	padding-left: 2px;
}

.score-input input {
	height: 50%;
	width: 45px;
	margin-left: 2px;
	margin-right: 3px;
}

.coordinates-view {
	display: flex;
	flex-direction: column;
	width: auto;
	height: 100%;
	border-style: solid;
	border-color: black;
	border-width: 1px;
	padding-left: 2px;
	padding-right: 2px;
}
</style>
