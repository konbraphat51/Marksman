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
					<td>{{ rowIndex + 1 }}</td>
					<td
						class="cell"
						v-for="(col, colIndex) in cols"
						:key="colIndex"
						@click="
							$emit('update:rowSelected', rowIndex),
								$emit('update:colSelected', colIndex)
						"
						:class="{
							selected: rowIndex === rowSelected && colIndex === colSelected,
						}"
					>
						{{ GetDatum(rowIndex, colIndex, "scoreX10") / 10 || 0 }}
					</td>
					<td
						class="cell total-row"
						@click="
							$emit('update:rowSelected', rowIndex),
								$emit('update:colSelected', -1)
						"
						:class="{
							selected: rowIndex === rowSelected && colSelected === -1,
						}"
					>
						{{ rowSums[rowIndex] / 10 || 0 }}
					</td>
				</tr>
				<tr>
					<td>Total</td>
					<td
						v-for="(col, colIndex) in cols"
						:key="colIndex"
						class="cell total-column"
						@click="
							$emit('update:rowSelected', -1),
								$emit('update:colSelected', colIndex)
						"
						:class="{
							selected: rowSelected === -1 && colSelected === colIndex,
						}"
					>
						{{ columnSums[colIndex] / 10 || 0 }}
					</td>
					<td
						class="cell total-all"
						@click="
							$emit('update:rowSelected', -1), $emit('update:colSelected', -1)
						"
						:class="{
							selected: rowSelected === -1 && colSelected === -1,
						}"
					>
						{{ totalSum / 10 || 0 }}
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
		colSelected: Number,
		rowSelected: Number,
	},
	data() {
		return {
			columnSums: [],
			rowSums: [],
			totalSum: 0,
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
		_UpdateSums() {
			this.columnSums = Array(this.cols).fill(0)
			this.rowSums = Array(this.rows).fill(0)
			this.totalSum = 0

			for (let r = 0; r < this.rows; r++) {
				for (let c = 0; c < this.cols; c++) {
					const scoreX10 = this.GetDatum(r, c, "scoreX10")
					if (scoreX10) {
						this.columnSums[c] += scoreX10
						this.rowSums[r] += scoreX10
						this.totalSum += scoreX10
					}
				}
			}
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
	background-color: yellow; /* Highlight selected cell */
}
</style>
