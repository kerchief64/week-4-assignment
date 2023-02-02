<script>
	export let files;
	$: console.log("files:", files)
</script>

<input
	bind:files
	accept="image/*"
	multiple
	id="image_picker"
	name="image_picker"
	type="file"
/>
