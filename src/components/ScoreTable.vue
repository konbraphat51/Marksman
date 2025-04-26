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
						0
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
						0
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
						0
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
						0
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
	emits: ["update:colSelected", "update:rowSelected"],
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
