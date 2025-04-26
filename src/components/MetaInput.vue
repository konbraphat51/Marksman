<template>
	<div class="MetaInput-container">
		<div class="meta-input__container">
			<div class="match-input">
				<label for="match">Match: </label>
				<input
					type="text"
					:value="metadata.match"
					@input="
						$emit('update:metadata', {...metadata, match: $event.target.value})
					"
				/>
			</div>
			<div class="date-input">
				<label for="date">Date: </label>
				<input
					type="date"
					:value="metadata.date"
					@input="
						$emit('update:metadata', {...metadata, date: $event.target.value})
					"
				/>
			</div>
			<div class="guntype-input">
				<label for="guntype">Gun Type: </label>

				<input
					type="radio"
					name="guntype"
					id="guntype"
					value="AR"
					:checked="metadata.guntype === 'AR'"
					@input="
						$emit('update:metadata', {
							...metadata,
							guntype: $event.target.value,
						})
					"
				/>
				<label for="airgun">AR</label>

				<input
					type="radio"
					name="guntype"
					id="guntype2"
					value="SB"
					:checked="metadata.guntype === 'SB'"
					@input="
						$emit('update:metadata', {
							...metadata,
							guntype: $event.target.value,
						})
					"
				/>
				<label for="shotgun">SB</label>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: "MetaInput",
	props: {
		metadata: Object,
	},
	emits: ["update:metadata"],
}
</script>
