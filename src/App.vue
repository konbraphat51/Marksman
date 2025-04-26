<template>
	<div class="upper-container">
		<div class="left">
			<Mark />
		</div>
		<div class="right">
			<div class="top">
				<Inputs
					:col="colSelected"
					:row="rowSelected"
					v-model:scoreX10="scoreX10"
				/>
			</div>
			<div class="bottom">
				<ScoreTable
					:dataShot="dataShot"
					:rows="rows"
					:cols="cols"
					v-model:rowSelected="rowSelected"
					v-model:colSelected="colSelected"
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
			dataInitialized: false,
			rows: 6,
			cols: 10,
			rowSelected: 0,
			colSelected: 0,
			scoreX10: 0,
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
						scoreX10: 0,
					})
				}
			}

			this.dataInitialized = true
		},

		_OnSelectionChanged() {
			this.scoreX10 = this.dataShot[this.rowSelected][this.colSelected].scoreX10
		},
	},
	watch: {
		scoreX10(newValue) {
			if (this.dataInitialized) {
				this.dataShot[this.rowSelected][this.colSelected].scoreX10 = newValue
			}
		},
		rowSelected(newValue) {
			this._OnSelectionChanged()
		},
		colSelected(newValue) {
			this._OnSelectionChanged()
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
