<script>
import Mark from "./components/Mark.vue"
import Inputs from "./components/Inputs.vue"
import ScoreTable from "./components/ScoreTable.vue"

export default {
	name: "App",
	components: {
		Mark,
		Inputs,
		ScoreTable,
	},
	data() {
		return {
			dataShot: [],
			rows: 6,
			cols: 10,
		}
	},
	mounted() {
		this._InitializeDataShot()
	},
	setup() {},
	methods: {
		_InitializeDataShot() {
			this.dataShot = []
			for (let r = 0; r < this.rows; r++) {
				//add a new row
				this.dataShot.push([])

				for (let c = 0; c < this.cols; c++) {
					// add a new column
					this.dataShot[r].push({
						set: false,
						x: 0,
						y: 0,
						score: 0,
					})
				}
			}
		},
	},
}
</script>

<template>
	<div class="upper-container">
		<div class="left">
			<Mark />
		</div>
		<div class="right">
			<div class="top">
				<Inputs />
			</div>
			<div class="bottom">
				<ScoreTable />
			</div>
		</div>
	</div>
</template>

<style scoped>
.upper-container {
	display: flex;
	width: 100%;
	height: auto;
}

.left {
	width: 34%;
	aspect-ratio: 1;
}

.right {
	display: flex;
	flex-direction: column;
	width: 66%;
}

.top {
	height: 20px;
}

.bottom {
	flex: 1;
}
</style>
