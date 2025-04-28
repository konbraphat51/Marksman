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
				@keydown.enter="$emit('update:scoreX10', $event.target.value * 10)"
				min="0"
				max="10.9"
				step="0.1"
			/>
		</div>
		<div class="ox-input">
			<div class="o">
				<input
					type="radio"
					name="ox"
					@input="$emit('update:ox', $event.target.value)"
					id="o"
					value="o"
					:checked="ox === 'o'"
				/>
				<label for="o">O</label>
			</div>

			<div class="x">
				<input
					type="radio"
					name="ox"
					@input="$emit('update:ox', $event.target.value)"
					id="x"
					value="x"
					:checked="ox === 'x'"
				/>
				<label for="x">X</label>
			</div>
		</div>
		<div class="coordinates-view">
			<div class="coordinates-value">X: {{ x.toFixed(3) }}</div>
			<div class="coordinates-value">Y: {{ y.toFixed(3) }}</div>
		</div>
		<div class="comment-input">
			<input
				type="text"
				:value="comment"
				@input="$emit('update:comment', $event.target.value)"
			/>
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
		ox: String,
		comment: String,
	},
	emits: ["update:scoreX10", "update:ox", "update:comment"],
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

.ox-input {
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

.comment-input {
	align-content: center;
	text-align: center;
	flex-grow: 1;
	height: 100%;
	border-style: solid;
	border-color: black;
	border-width: 1px;
}

.comment-input input {
	width: 90%;
	height: 70%;
}
</style>
