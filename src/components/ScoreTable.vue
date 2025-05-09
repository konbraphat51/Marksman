<template>
	<div class="ScoreTable-container">
		<table class="ScoreTable">
			<thead>
				<tr>
					<th></th>
					<th v-for="(col, index) in cols" :key="index">{{ index + 1 }}</th>
					<th>Total</th>
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
						{{
							(GetDatum(rowIndex, colIndex, "scoreX10") / 10).toFixed(1) || 0
						}}
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
						{{ (rowSums[rowIndex] / 10).toFixed(1) || 0 }}
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
						{{ (columnSums[colIndex] / 10).toFixed(1) || 0 }}
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
						{{ (totalSum / 10).toFixed(1) || 0 }}
					</td>
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
		colSelected: Number,
		rowSelected: Number,
	},
	data() {
		return {
			columnSums: [],
			rowSums: [],
			totalSum: 0,
			highlight: [],
		}
	},
	emits: ["update:colSelected", "update:rowSelected"],
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
			if (this.prepRows.includes(rowIndex)) {
				// is preprow
				switch (this.prepRows.length) {
					case 2:
						return `Preparation ${rowIndex + 1}`
					case 6:
						let alphabets = ["K", "P", "S"]

						return `Preparation ${alphabets[Math.floor(rowIndex / 4)]}${
							rowIndex % 2 === 0 ? "1" : "2"
						}`
				}
			} else {
				// is not preprow
				let less = 0
				for (let cnt = 0; cnt < this.prepRows.length; cnt++) {
					if (this.prepRows[cnt] < rowIndex) {
						less++
					}
				}

				return `${rowIndex - less + 1}`
			}
		},
		_UpdateSums() {
			this.columnSums = Array(this.cols).fill(0)
			this.rowSums = Array(this.rows).fill(0)
			this.totalSum = 0

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
						this.rowSums[r] += scoreX10

						if (!prep) {
							this.totalSum += scoreX10
							this.columnSums[c] += scoreX10
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
