<template>
	<div class="upper-container">
		<div class="left">
			<Mark />
		</div>
		<div class="right">
			<div class="top">
				<Inputs :col="colSelected" :row="rowSelected" />
			</div>
			<div class="bottom">
				<ScoreTable
					:dataShot="dataShot"
					:rows="rows"
					:cols="cols"
					@update:rowSelected="(val) => (rowSelected = val)"
					@update:colSelected="(val) => (colSelected = val)"
				/>
			</div>
		</div>
	</div>
</template>

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
			rowSelected: 0,
			colSelected: 0,
		}
	},
	mounted() {
		this._InitializeDataShot()
	},
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
	height: 35px;
}

.bottom {
	flex: 1;
}
</style>
