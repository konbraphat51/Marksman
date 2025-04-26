<template>
	<div class="upper-container">
		<div class="left">
			<Mark
				@clickedCoordinates="
					(coordinates) => {
						this.selectedX = coordinates.x
						this.selectedY = coordinates.y

						this.dataShot[this.rowSelected][this.colSelected].x = coordinates.x
						this.dataShot[this.rowSelected][this.colSelected].y = coordinates.y
						this.dataShot[this.rowSelected][this.colSelected].set = true
					}
				"
				:dataShot="dataShot"
				:rowSelected="rowSelected"
				:colSelected="colSelected"
				:colors="colors"
			/>
		</div>
		<div class="right">
			<div class="top">
				<Inputs
					:col="colSelected"
					:row="rowSelected"
					:x="selectedX"
					:y="selectedY"
					:scoreX10="scoreX10Selected"
					@update:scoreX10="
						(score) => {
							scoreX10Selected = score
							this.dataShot[this.rowSelected][this.colSelected].scoreX10 = score
						}
					"
					:ox="oxSelected"
					@update:ox="
						(ox) => {
							oxSelected = ox
							this.dataShot[this.rowSelected][this.colSelected].ox = ox
						}
					"
					:comment="commentSelected"
					@update:comment="
						(comment) => {
							this.commentSelected = comment
							this.dataShot[this.rowSelected][this.colSelected].comment =
								comment
						}
					"
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
	<ColorPicker v-model:colors="colors" />
</template>

<script>
import Mark from "./components/Mark.vue"
import Inputs from "./components/Inputs.vue"
import ScoreTable from "./components/ScoreTable.vue"
import ColorPicker from "./components/ColorPicker.vue"

export default {
	name: "App",
	components: {
		Mark,
		Inputs,
		ScoreTable,
		ColorPicker,
	},
	data() {
		return {
			dataShot: [],
			rows: 6,
			cols: 10,
			rowSelected: 0,
			colSelected: 0,
			scoreX10Selected: 0,
			oxSelected: "",
			selectedX: 0,
			selectedY: 0,
			commentSelected: "",
			colors: [
				"#FF0000",
				"#00FF00",
				"#0000FF",
				"#FFFF00",
				"#FF00FF",
				"#00FFFF",
				"#FFFFFF",
				"#000000",
				"#808080",
				"#FFA500",
			],
		}
	},
	beforeMount() {
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
						ox: "",
						comment: "",
					})
				}
			}
		},

		_OnSelectionChanged() {
			if (this.rowSelected !== -1 && this.colSelected !== -1) {
				this.scoreX10Selected =
					this.dataShot[this.rowSelected][this.colSelected].scoreX10
				this.selectedX = this.dataShot[this.rowSelected][this.colSelected].x
				this.selectedY = this.dataShot[this.rowSelected][this.colSelected].y
				this.oxSelected = this.dataShot[this.rowSelected][this.colSelected].ox
				this.commentSelected =
					this.dataShot[this.rowSelected][this.colSelected].comment
			} else {
				this.scoreX10Selected = 0
				this.selectedX = 0
				this.selectedY = 0
				this.oxSelected = ""
				this.commentSelected = ""
			}
		},
	},
	watch: {
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
	height: 40px;
}

.bottom {
	flex: 1;
}
</style>
