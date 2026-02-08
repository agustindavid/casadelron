<script lang="ts">
	let { id, selectedState, d } = $props();
</script>

<path class="st1" class:selected={selectedState === id} {id} {d} />

<style>
	.st0 {
		fill: none;
	}

	.st1 {
		stroke: #444;
		stroke-miterlimit: 11;
		stroke-width: 0.5px;
	}

	.st1,
	.st2 {
		fill: #f6f3ec;
		transition: all 0.2s ease-in-out;
	}

	.selected {
		fill: rgb(0, 0, 0) !important;
	}
</style>
