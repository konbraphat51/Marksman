<script setup>
import Mark from "./components/Mark.vue"
import Inputs from "./components/Inputs.vue"
import ScoreTable from "./components/ScoreTable.vue"
</script>

<template>
	<div class="upper-container">
		<div class="left">
			<Mark />
		</div>
		<div class="right">
			<div class="top">
				<Inputs />
			</div>
			<div class="bottom">
				<ScoreTable />
			</div>
		</div>
	</div>
</template>

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
	height: 20px;
}

.bottom {
	flex: 1;
}
</style>
