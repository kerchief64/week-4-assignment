<script>
  import { onDestroy } from "svelte";
	export let src;
	export let width;
	export let height;
  export let selected;

  onDestroy(() => {
		console.log("An image was deleted")
	});
</script>

<img
  class:markedForDelete={selected}
	src={src}
	width="{width}px"
	height="{height}px"
	alt="Gallery"
/>

<style>
  .markedForDelete {
    opacity: 50%;
  }
</style>