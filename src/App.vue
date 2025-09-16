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
				:guntype="metadata.guntype"
				:multipleSelection="multipleSelection"
			/>
		</div>
		<div class="right">
			<div class="top">
				<Inputs
					v-if="this.colSelected !== -1 && this.rowSelected !== -1"
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
				<InputsSum
					v-else
					:col="colSelected"
					:row="rowSelected"
					:dataShot="dataShot"
					:comment="commentSelected"
					@update:comment="
						(comment) => {
							this.commentSelected = comment
							this.sumComments[
								this._GetSumIndex(this.rowSelected, this.colSelected)
							] = comment
						}
					"
				/>
			</div>
			<div class="bottom">
				<ScoreTable
					:dataShot="dataShot"
					:rows="rows"
					:cols="cols"
					:prepRows="prepRows"
					:guntype="metadata.guntype"
					v-model:rowSelected="rowSelected"
					v-model:colSelected="colSelected"
					:timesFinished="timesFinished"
					:metadata="metadata"
					:multipleSelection="multipleSelection"
					@update:timesFinished="timesFinished = $event"
					@onSelectedMultiple="this.multipleSelection.push($event)"
					@onSelected="this.multipleSelection = []"
				/>
			</div>
		</div>
	</div>
	<MetaInput v-model:metadata="metadata" />
	<FileControl
		:entireData="entireData"
		@upload="
			(fileData) => {
				this.DecompressJson(fileData)
			}
		"
	/>
	<ColorPicker v-model:colors="colors" />
</template>

<script>
import Mark from "./components/Mark.vue"
import Inputs from "./components/Inputs.vue"
import ScoreTable from "./components/ScoreTable.vue"
import ColorPicker from "./components/ColorPicker.vue"
import InputsSum from "./components/InputsSum.vue"
import MetaInput from "./components/MetaInput.vue"
import FileControl from "./components/FileControl.vue"

export default {
	name: "App",
	components: {
		Mark,
		Inputs,
		ScoreTable,
		ColorPicker,
		InputsSum,
		MetaInput,
		FileControl,
	},
	data() {
		return {
			dataShot: [],
			prepRows: [0, 1],
			rows: 8,
			cols: 10,
			rowSelected: 0,
			colSelected: 0,
			scoreX10Selected: 0,
			oxSelected: "",
			selectedX: 0,
			selectedY: 0,
			commentSelected: "",
			sumComments: [], // row -> column -> all
			colors: [
				"#0000FF", // 0.0-0.9
				"#0000FF", // 1.0-1.9
				"#0000FF", // 2.0-2.9
				"#0000FF", // 3.0-3.9
				"#0000FF", // 4.0-4.9
				"#0000FF", // 5.0-5.9
				"#0000FF", // 6.0-6.9
				"#0000FF", // 7.0-7.9
				"#0000FF", // 8.0-8.9
				"#FFA500", // 9.0-9.9
				"#FF0000", // 10.0-10.9
			],
			metadata: {
				match: "",
				date: "",
				time: "",
				guntype: "AR",
			},
			timesFinished: new Array(this.rows).fill(""),
			multipleSelection: [], // for ctrl+click
		}
	},
	created() {
		this._InitializeDataShot()
		this._InitializeSumComments()
		this._InitializeTimesFinished()
	},
	methods: {
		DecompressJson(json) {
			this.metadata = json.metadata
			this.dataShot = json.dataShot
			this.colors = json.colors
			this.sumComments = json.sumComments
			this.timesFinished = json.timesFinished
		},
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
		_InitializeSumComments() {
			this.sumComments = new Array(this.rows + this.cols + 1).fill("")
		},
		_InitializeTimesFinished() {
			this.timesFinished = new Array(this.rows).fill("")
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
				this.commentSelected =
					this.sumComments[
						this._GetSumIndex(this.rowSelected, this.colSelected)
					]
			}
		},
		_GetSumIndex(row, col) {
			if (row === -1 && col !== -1) {
				return this.rows + col
			} else if (row !== -1 && col === -1) {
				return row
			} else {
				return this.rows + this.cols
			}
		},
	},
	computed: {
		entireData() {
			return {
				metadata: this.metadata,
				dataShot: this.dataShot,
				colors: this.colors,
				sumComments: this.sumComments,
				timesFinished: this.timesFinished,
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
		"metadata.guntype": function (newValue) {
			switch (newValue) {
				case "AR":
				case "P60":
					this.rows = 8
					this.prepRows = [0, 1]
					break
				case "3x20":
					this.rows = 12
					this.prepRows = [0, 1, 4, 5, 8, 9]
					break
			}
		},
		rows(newValue) {
			this._InitializeDataShot()
			this._InitializeTimesFinished()
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
	height: 60px;
}

.bottom {
	flex: 1;
}
</style>
