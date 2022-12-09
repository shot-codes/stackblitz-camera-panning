<script lang="ts">
	import { Canvas } from '@threlte/core';
	import Scene from '$lib/Scene.svelte';
</script>

<div>
	<Canvas>
		<Scene />
	</Canvas>
</div>

<style>
	div {
		height: 100vh;
		width: 100vw;
	}
</style>
