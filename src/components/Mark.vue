<template>
	<div class="mark-container">
		<svg xmlns="http://www.w3.org/2000/svg" @click="_OnClicked">
			<!-- area -->
			<circle
				v-for="(radius, index) in radiuses"
				:r="radius + '%'"
				class="area"
				cx="50%"
				cy="50%"
			/>

			<!-- border -->
			<circle
				v-for="(radius, index) in radiuses"
				:r="radius + '%'"
				class="border"
				cx="50%"
				cy="50%"
			/>

			<!-- area number -->
			<svg v-for="(radius, index) in radiuses" :key="index">
				<text
					v-if="radius > 10"
					font-size="10"
					:x="50 - radius + 2 + '%'"
					y="51%"
				>
					{{ index + 1 }}
				</text>
			</svg>

			<!-- bullet hole for selected -->
			<BulletHole
				v-for="(hole, index) in holes"
				:x="hole.x"
				:y="hole.y"
				:color="_SelectColor(hole.scoreX10)"
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
		colors: Array,
		guntype: String,
	},
	data() {
		return {
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
		_UpdateHoles() {
			let holes = []
			try {
				if (this.rowSelected !== -1 && this.colSelected !== -1) {
					if (this.dataShot[this.rowSelected][this.colSelected].set) {
						holes.push(this.dataShot[this.rowSelected][this.colSelected])
					}
				} else if (this.rowSelected === -1 && this.colSelected !== -1) {
					for (let r = 0; r < this.dataShot.length; r++) {
						if (this.dataShot[r][this.colSelected].set) {
							holes.push(this.dataShot[r][this.colSelected])
						}
					}
				} else if (this.rowSelected !== -1 && this.colSelected === -1) {
					for (let c = 0; c < this.dataShot[0].length; c++) {
						if (this.dataShot[this.rowSelected][c].set) {
							holes.push(this.dataShot[this.rowSelected][c])
						}
					}
				} else {
					for (let r = 0; r < this.dataShot.length; r++) {
						for (let c = 0; c < this.dataShot[0].length; c++) {
							if (this.dataShot[r][c].set) {
								holes.push(this.dataShot[r][c])
							}
						}
					}
				}
			} catch (e) {
				holes = []
			}
			this.holes = holes
		},
		_SelectColor(scoreX10) {
			return this.colors[parseInt(scoreX10 / 10)]
		},
	},
	computed: {
		radiuses() {
			if (this.guntype === "AR") {
				return [
					95, //1
					85, //2
					75, //3
					65, //4
					55, //5
					45, //6
					35, //7
					25, //8
					10, //9
					2, //10
				]
			} else if (this.guntype === "3x20" || this.guntype === "P60") {
				return [
					95, //1
					85, //2
					75, //3
					65, //4
					55, //5
					45, //6
					35, //7
					25, //8
					15, //9
					6, //10
					3, //11
				]
			} else {
				return [45, 25, 5, 1]
			}
		},
	},
	watch: {
		dataShot: {
			handler() {
				this._UpdateHoles()
			},
			deep: true,
		},
		rowSelected() {
			this._UpdateHoles()
		},
		colSelected() {
			this._UpdateHoles()
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
