<template>
	<button @click="UploadFile">Upload</button>
	<button @click="DownloadFile">Download</button>
</template>

<script>
export default {
	name: "FileControl",
	props: {
		entireData: Object,
	},
	emits: ["upload"],
	methods: {
		UploadFile() {
			const fileInput = document.createElement("input")
			fileInput.type = "file"
			fileInput.accept = ".json"

			fileInput.onchange = (event) => {
				const file = event.target.files[0]
				const reader = new FileReader()
				reader.onload = (e) => {
					const json = JSON.parse(e.target.result)
					this.$emit("upload", json)
				}
				reader.readAsText(file)
			}
			fileInput.click()
		},

		DownloadFile() {
			const fileName = `${this.entireData.metadata.match}_${this.entireData.metadata.guntype}.json`

			const dataStr = JSON.stringify(this.entireData, null, 2)
			const blob = new Blob([dataStr], {type: "application/json"})
			const url = URL.createObjectURL(blob)
			const a = document.createElement("a")
			a.href = url
			a.download = fileName
			a.click()
			URL.revokeObjectURL(url)
		},
	},
}
</script>
