<template>
	<div class="colorPicker-container">
		<div class="colorPicker" v-for="(color, index) in colorsHere" :key="index">
			<label for="picker">
				{{ index.toFixed(1) }} - {{ (index + 0.9).toFixed(1) }}
			</label>
			<input
				type="color"
				:value="color"
				@input="
					(event) => {
						this.colorsHere[index] = event.target.value
						this.$emit('update:colors', this.colorsHere)
					}
				"
			/>
		</div>
	</div>
</template>

<script>
export default {
	name: "ColorPicker",
	props: {
		colors: Array,
	},
	emits: ["update:colors"],
	data() {
		return {
			colorsHere: [],
		}
	},
	mounted() {
		this.colorsHere = this.colors
	},
	watch: {
		colors: {
			deep: true,
			handler() {
				console.log(this.colors)
				this.colorsHere = this.colors
			},
		},
	},
}
</script>
