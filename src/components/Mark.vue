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

			<!-- bullet hole for selected -->
			<BulletHole
				v-for="(hole, index) in holes"
				:key="index"
				:x="hole.x"
				:y="hole.y"
			/>
		</svg>
	</div>
</template>

<script>
import BulletHole from "./BulletHole.vue"

export default {
	name: "Mark",
	components: {
		BulletHole,
	},
	props: {
		dataShot: Array,
		rowSelected: Number,
		colSelected: Number,
	},
	data() {
		return {
			radius: [45, 41, 37, 33, 29, 25, 21, 17, 13, 9, 5],
			holes: [],
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
	watch: {
		dataShot: {
			handler() {
				try {
					if (this.rowSelected !== -1 && this.colSelected !== -1) {
						this.holes = this.dataShot[this.rowSelected][this.colSelected]
					} else if (this.rowSelected === -1 && this.colSelected !== -1) {
						this.holes = []
						for (let r = 0; r < this.dataShot.length; r++) {
							this.holes.push(this.dataShot[r][this.colSelected])
						}
					} else if (this.rowSelected !== -1 && this.colSelected === -1) {
						this.holes = []
						for (let c = 0; c < this.dataShot[0].length; c++) {
							this.holes.push(this.dataShot[this.rowSelected][c])
						}
					} else {
						this.holes = []
						for (let r = 0; r < this.dataShot.length; r++) {
							for (let c = 0; c < this.dataShot[0].length; c++) {
								this.holes.push(this.dataShot[r][c])
							}
						}
					}
				} catch (e) {
					this.holes = []
				}
			},
			deep: true,
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
