<template>
	<div class="MetaInput-container">
		<div class="meta-input__container">
			<div class="match-input">
				<label for="match">Match: </label>
				<input
					type="text"
					:value="metadata.match"
					@input="
						$emit('update:metadata', {
							...metadata,
							match: $event.target.value,
						})
					"
				/>
			</div>
			<div class="date-input">
				<label for="date">Date: </label>
				<input
					type="date"
					:value="metadata.date"
					@input="
						$emit('update:metadata', { ...metadata, date: $event.target.value })
					"
				/>
			</div>
			<div class="time-input">
				<label for="time">Time Started: </label>
				<input
					type="time"
					:value="metadata.time"
					@input="
						$emit('update:metadata', { ...metadata, time: $event.target.value })
					"
				/>
			</div>
			<div class="guntype-input">
				<label for="guntype">Gun Type: </label>

				<input
					type="radio"
					name="guntype"
					id="guntypeAR"
					value="AR"
					:checked="metadata.guntype === 'AR'"
					@input="
						$emit('update:metadata', {
							...metadata,
							guntype: $event.target.value,
						})
					"
				/>
				<label for="AR">AR</label>

				<input
					type="radio"
					name="guntype"
					id="guntype3x20"
					value="3x20"
					:checked="metadata.guntype === '3x20'"
					@input="
						$emit('update:metadata', {
							...metadata,
							guntype: $event.target.value,
						})
					"
				/>
				<label for="3x20">3x20</label>

				<input
					type="radio"
					name="guntype"
					id="guntypeP60"
					value="P60"
					:checked="metadata.guntype === 'P60'"
					@input="
						$emit('update:metadata', {
							...metadata,
							guntype: $event.target.value,
						})
					"
				/>
				<label for="P60">P60</label>
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
