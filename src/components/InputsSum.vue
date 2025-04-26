<template>
	<div class="input-container">
		<div class="index-view">
			<div class="col-view" v-if="col !== -1">col: {{ col + 1 }}</div>
			<div class="row-view" v-if="row !== -1">row: {{ row + 1 }}</div>
		</div>
		<div class="ox-view">
			<div class="o">O: {{ o }}</div>
			<div class="x">X: {{ x }}</div>
		</div>
		<div class="comment-input">
			<input
				type="text"
				:value="comment"
				@input="$emit('update:comment', $event.target.value)"
			/>
		</div>
	</div>
</template>

<script>
export default {
	name: "InputsSum",
	props: {
		col: Number,
		row: Number,
		dataShot: Array,
		comment: String,
	},
	emits: ["update:comment"],
	data() {
		return {
			o: 0,
			x: 0,
		}
	},
	mounted() {
		this.CountOx()
	},
	methods: {
		ListupData() {
			if (this.col === -1 && this.row !== -1) {
				// sum of row
				let result = []
				for (let c = 0; c < this.dataShot[0].length; c++) {
					result.push(this.dataShot[this.row][c])
				}
				return result
			} else if (this.col !== -1 && this.row === -1) {
				// sum of col
				let result = []
				for (let r = 0; r < this.dataShot.length; r++) {
					result.push(this.dataShot[r][this.col])
				}
				return result
			} else {
				// sum of all
				let result = []
				for (let r = 0; r < this.dataShot.length; r++) {
					for (let c = 0; c < this.dataShot[0].length; c++) {
						result.push(this.dataShot[r][c])
					}
				}
				return result
			}
		},
		CountOx() {
			const data = this.ListupData()

			this.o = 0
			this.x = 0

			for (let cnt = 0; cnt < data.length; cnt++) {
				if (data[cnt].ox === "o") {
					this.o++
				} else if (data[cnt].ox === "x") {
					this.x++
				}
			}
		},
	},
	watch: {
		dataShot: {
			handler() {
				this.CountOx()
			},
			deep: true,
		},
	},
}
</script>

<style scoped>
.input-container {
	display: flex;
	flex-direction: row;
	width: 100%;
	height: 100%;
	background-color: aqua;
}

.index-view {
	width: auto;
	height: 100%;
	border-width: 1px;
	border-style: solid;
	border-color: black;
	padding-right: 5px;
	padding-left: 2px;
}

.ox-view {
	display: flex;
	flex-direction: column;
	width: auto;
	height: 100%;
	border-style: solid;
	border-color: black;
	border-width: 1px;
	padding-left: 2px;
	padding-right: 2px;
}

.comment-input {
	align-content: center;
	text-align: center;
	flex-grow: 1;
	height: 100%;
	border-style: solid;
	border-color: black;
	border-width: 1px;
}

.comment-input input {
	width: 90%;
	height: 70%;
}
</style>
