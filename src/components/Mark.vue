<template>
	<div class="mark-container">
		<svg xmlns="http://www.w3.org/2000/svg" @click="_OnClicked">
			<!-- area -->
			<circle
				v-for="(radius, index) in radius"
				:r="radius + '%'"
				class="area"
				cx="50%"
				cy="50%"
			/>

			<!-- border -->
			<circle
				v-for="(radius, index) in radius"
				:r="radius + '%'"
				class="border"
				cx="50%"
				cy="50%"
			/>
		</svg>
	</div>
</template>

<script>
export default {
	name: "Mark",
	data() {
		return {
			radius: [45, 41, 37, 33, 29, 25, 21, 17, 13, 9, 5],
		}
	},
	emits: ["clickedCoordinates"],
	methods: {
		_OnClicked(event) {
			const svg = event.currentTarget
			const rect = svg.getBoundingClientRect()
			const xRelative = (event.clientX - rect.left) / rect.width
			const yRelative = (event.clientY - rect.top) / rect.height
			this.$emit("clickedCoordinates", {x: xRelative, y: yRelative})
		},
	},
}
</script>

<style scoped>
.mark-container {
	display: flex;
	justify-content: center;
	align-items: center;
	width: 100%;
	height: 100%;
	background-color: burlywood;
}

svg {
	width: 100%;
	height: 100%;
}

.area {
	fill: lightblue;
	stroke: none;
	transition: transform 0.2s;
}

.area:hover {
	fill: red;
	transition: fill 0.2s;
}

.border {
	fill: none;
	stroke: black;
	stroke-width: 2;
	pointer-events: none;
}
</style>
