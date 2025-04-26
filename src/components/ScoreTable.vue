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
				<tr v-for="(row, index) in rows" :key="index">
					<td>{{ index + 1 }}</td>
					<td v-for="(col, colIndex) in cols" :key="colIndex">0</td>
					<td>0</td>
				</tr>
				<tr>
					<td>Total</td>
					<td v-for="(col, colIndex) in cols" :key="colIndex">0</td>
					<td>0</td>
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
</style>
