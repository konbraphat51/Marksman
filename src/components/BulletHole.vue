<template>
	<svg
		:width="sizePercent"
		:height="sizePercent"
		:x="leftTopX"
		:y="leftTopY"
		viewBox="0 0 10 10"
		xmlns="http://www.w3.org/2000/svg"
	>
		<line x1="0" y1="0" x2="10" y2="10" stroke="black" stroke-width="1" />
		<line x1="10" y1="0" x2="0" y2="10" stroke="black" stroke-width="1" />
	</svg>
</template>

<script>
export default {
	name: "BulletHole",
	props: {
		x: Number,
		y: Number,
	},
	data() {
		return {
			size: 6, //%
		}
	},
	methods: {},
	computed: {
		leftTopX() {
			let x = this.x * 100 - this.size / 2
			console.log("leftTopX", this.x)
			return `${x}%`
		},
		leftTopY() {
			let y = this.y * 100 - this.size / 2
			return `${y}%`
		},
		rightBottomX() {
			let x = this.x * 100 + this.size / 2
			return `${x}%`
		},
		rightBottomY() {
			let y = this.y * 100 + this.size / 2
			return `${y}%`
		},
		sizePercent() {
			return `${this.size}%`
		},
	},
}
</script>
