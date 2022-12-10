<script lang="ts">
	import { T, TransformableObject, useThrelte } from '@threlte/core';
	import { Color, Mesh, Vector3 } from 'three';
	import { spring } from 'svelte/motion';

	const { pointer } = useThrelte();

	let rawCameraPosition = [10, 10, 0];
	const cameraPosition = new Vector3(10, 10, 0);
	const cameraOrigin = new Vector3(10, 10, 0);
	let mesh: Mesh;

	// const pointerSpring = spring($pointer);
	const cameraPositionSpring = spring(rawCameraPosition);
	// $: {
	// 	pointerSpring.set($pointer);
	// }
	$: {
		cameraPositionSpring.set([
			cameraOrigin.x,
			cameraOrigin.y + 8 * $pointer.y,
			cameraOrigin.z - 8 * $pointer.x
		]);
	}
	$: {
		cameraPosition.set(
			$cameraPositionSpring[0],
			$cameraPositionSpring[1],
			$cameraPositionSpring[2]
		);
		console.log(cameraPosition);
	}
</script>

<T.PerspectiveCamera let:ref makeDefaultfov={50}>
	<TransformableObject object={ref} lookAt={mesh} position={cameraPosition} />
</T.PerspectiveCamera>

<T.DirectionalLight castShadow position={[3, 10, 10]} />
<T.DirectionalLight position={[-3, 10, -10]} intensity={0.2} />
<T.AmbientLight intensity={0.2} />

<T.Group>
	<T.Mesh bind:ref={mesh} position.y={0.5} castShadow let:ref>
		<T.BoxGeometry />
		<T.MeshStandardMaterial color="#333333" />
	</T.Mesh>
</T.Group>
