<template>
	<g>
		<line 
			:x1="`${x * 100}%`" 
			:y1="`${(y * 100) - 3}%`" 
			:x2="`${x * 100}%`" 
			:y2="`${(y * 100) + 3}%`" 
			:stroke="color" 
			stroke-width="1" 
		/>
		<line 
			:x1="`${(x * 100) - 3}%`" 
			:y1="`${y * 100}%`" 
			:x2="`${(x * 100) + 3}%`" 
			:y2="`${y * 100}%`" 
			:stroke="color" 
			stroke-width="1" 
		/>
	</g>
</template>

<script>
export default {
	name: "BulletHole",
	props: {
		x: Number,
		y: Number,
		color: String,
	},
}
</script>
