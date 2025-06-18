<template>
	<div class="ScoreTable-container">
		<table class="ScoreTable">
			<thead>
				<tr>
					<th></th>
					<th v-for="(col, index) in cols" :key="index">{{ index + 1 }}</th>
					<th>Total</th>
					<th>Time Finished</th>
					<th>Time Diff (mins)</th>
				</tr>
			</thead>
			<tbody>
				<tr v-for="(row, rowIndex) in rows" :key="rowIndex">
					<td>{{ GetRowTitle(rowIndex) }}</td>
					<td
						class="cell"
						v-for="(col, colIndex) in cols"
						:key="colIndex"
						@click.left="
							$emit('update:rowSelected', rowIndex),
								$emit('update:colSelected', colIndex)
						"
						@click.right.prevent="this._ToggleHighlight(rowIndex, colIndex)"
						:class="{
							selected: rowIndex === rowSelected && colIndex === colSelected,
							highlighted: this._IsHighlighted(rowIndex, colIndex),
						}"
					>
						{{ ShowScore(this.GetDatum(rowIndex, colIndex, "scoreX10")) }}
					</td>
					<td
						class="cell total-row"
						@click.left="
							$emit('update:rowSelected', rowIndex),
								$emit('update:colSelected', -1)
						"
						:class="{
							selected: rowIndex === rowSelected && colSelected === -1,
						}"
					>
						{{ ShowRowSum(rowIndex) }}
					</td>
					<td class="cell time-finished">
						<input
							type="time"
							step="10"
							:value="timesFinished[rowIndex]"
							@input="onTimeInput($event, rowIndex)"
						/>
					</td>
					<td class="cell time-diff">
						{{ getTimeDiff(rowIndex) }}
					</td>
				</tr>
				<tr>
					<td>Total</td>
					<td
						v-for="(col, colIndex) in cols"
						:key="colIndex"
						class="cell total-column"
						@click.left="
							$emit('update:rowSelected', -1),
								$emit('update:colSelected', colIndex)
						"
						:class="{
							selected: rowSelected === -1 && colSelected === colIndex,
						}"
					>
						{{ ShowColumnSum(colIndex) }}
					</td>
					<td
						class="cell total-all"
						@click.left="
							$emit('update:rowSelected', -1), $emit('update:colSelected', -1)
						"
						:class="{
							selected: rowSelected === -1 && colSelected === -1,
						}"
					>
						{{ ShowTotalSum() }}
					</td>
					<td class="cell time-finished"></td>
					<td class="cell time-diff"></td>
				</tr>
			</tbody>
		</table>
	</div>
</template>

<script>
export default {
	name: "ScoreTable",
	props: {
		dataShot: Array,
		cols: Number,
		rows: Number,
		prepRows: Array,
		guntype: String,
		colSelected: Number,
		rowSelected: Number,
		timesFinished: Array,
	},
	emits: ["update:colSelected", "update:rowSelected", "update:timesFinished"],
	data() {
		return {
			columnSums: [],
			rowSums: [],
			totalSum: 0,
			highlight: [],
		}
	},
	beforeMount() {
		this._UpdateSums()
	},
	methods: {
		GetDatum(row, col, key) {
			try {
				return this.dataShot[row][col][key]
			} catch (e) {
				return null
			}
		},
		GetRowTitle(rowIndex) {
			switch (this.guntype) {
				case "AR":
				case "P60":
					if (this.prepRows.includes(rowIndex)) {
						return `Preparation ${rowIndex + 1}`
					} else {
						return `${rowIndex - this.prepRows.length + 1}`
					}

				case "3x20":
					const alphabet = ["K", "P", "S"][Math.floor(rowIndex / 4)]
					const number = (rowIndex % 2) + 1
					if (this.prepRows.includes(rowIndex)) {
						return `Preparation ${alphabet}${number}`
					} else {
						return `${alphabet}${number}`
					}
			}
		},
		_UpdateSums() {
			this.columnSums = this._FillSumList(this.cols)
			this.rowSums = this._FillSumList(this.rows)
			this.totalSum = [0, 0]

			let prep

			for (let r = 0; r < this.rows; r++) {
				if (this.prepRows.includes(r)) {
					prep = true
				} else {
					prep = false
				}

				for (let c = 0; c < this.cols; c++) {
					const scoreX10 = this.GetDatum(r, c, "scoreX10")
					if (scoreX10) {
						this.rowSums[r][0] += scoreX10
						this.rowSums[r][1] += Math.floor(scoreX10 / 10)

						if (!prep) {
							this.columnSums[c][0] += scoreX10
							this.columnSums[c][1] += Math.floor(scoreX10 / 10)

							this.totalSum[0] += scoreX10
							this.totalSum[1] += Math.floor(scoreX10 / 10)
						}
					}
				}
			}
		},
		_ToggleHighlight(row, col) {
			const index = this.highlight.findIndex(
				(item) => item[0] === row && item[1] === col,
			)
			if (index !== -1) {
				this.highlight.splice(index, 1)
			} else {
				this.highlight.push([row, col])
				console.log(this.highlight)
			}
		},
		_IsHighlighted(row, col) {
			return this.highlight.some((item) => item[0] === row && item[1] === col)
		},
		_FillSumList(length) {
			const list = []
			for (let cnt = 0; cnt < length; cnt++) {
				list.push([0, 0])
			}
			return list
		},
		ShowScore(score) {
			if (score === undefined) {
				return "x"
			}

			switch (this.guntype) {
				case "AR":
				case "P60":
					return `${(score / 10).toFixed(1)}`
				case "3x20":
					return `${Math.floor(score / 10)} (${(score / 10).toFixed(1)})`
			}
		},
		ShowRowSum(rowIndex) {
			try {
				switch (this.guntype) {
					case "AR":
					case "P60":
						return `${(this.rowSums[rowIndex][0] / 10).toFixed(1)}`
					case "3x20":
						return `${this.rowSums[rowIndex][1]} (${(
							this.rowSums[rowIndex][0] / 10
						).toFixed(1)})`
				}
			} catch (e) {
				return ""
			}
		},
		ShowColumnSum(colIndex) {
			try {
				switch (this.guntype) {
					case "AR":
					case "P60":
						return `${(this.columnSums[colIndex][0] / 10).toFixed(1)}`
					case "3x20":
						return `${this.columnSums[colIndex][1]} (${(
							this.columnSums[colIndex][0] / 10
						).toFixed(1)})`
				}
			} catch (e) {
				return ""
			}
		},
		ShowTotalSum() {
			try {
				switch (this.guntype) {
					case "AR":
					case "P60":
						return `${(this.totalSum[0] / 10).toFixed(1)}`
					case "3x20":
						return `${this.totalSum[1]} (${(this.totalSum[0] / 10).toFixed(1)})`
				}
			} catch (e) {
				return ""
			}
		},
		onTimeInput(event, rowIndex) {
			const newTimes = this.timesFinished.slice()
			newTimes[rowIndex] = event.target.value
			this.$emit("update:timesFinished", newTimes)
		},
		getTimeDiff(rowIndex) {
			if (rowIndex === 0) return ""
			const prev = this.timesFinished[rowIndex - 1]
			const curr = this.timesFinished[rowIndex]
			if (!prev || !curr) return ""
			// Parse as HH:mm or HH:mm:ss
			const parse = (t) => {
				if (!t) return null
				const parts = t.split(":").map(Number)
				if (parts.length < 2) return null
				return parts[0] * 3600 + parts[1] * 60 + (parts[2] || 0)
			}
			const prevSec = parse(prev)
			const currSec = parse(curr)
			if (prevSec == null || currSec == null) return ""
			const diff = currSec - prevSec
			if (diff < 0) return ""
			const mm = Math.floor(diff / 60)
			const ss = diff % 60
			return `${mm.toString()}:${ss.toString().padStart(2, "0")}`
		},
	},
	watch: {
		dataShot: {
			handler() {
				this._UpdateSums()
			},
			deep: true,
		},
	},
}
</script>

<style scoped>
.ScoreTable-container {
	width: 100%;
	height: 100%;
	background-color: lightgreen;
	display: flex;
	justify-content: center;
	align-items: center;
}

.ScoreTable {
	width: 100%;
	height: 100%;
	border-collapse: collapse;
}

.ScoreTable th,
.ScoreTable td {
	border: 1px solid black;
	text-align: center;
}

.ScoreTable td.selected {
	background-color: yellow;
}

.ScoreTable td.highlighted {
	background-color: rgb(248, 181, 228);
}
</style>
