<script lang="ts">
	import { T, TransformableObject, useThrelte } from '@threlte/core';
	import { Mesh, Vector3 } from 'three';
	import { spring } from 'svelte/motion';

	const { pointer } = useThrelte();
	let cameraPosition = new Vector3(10, 10, 0);
	const cameraOrigin = cameraPosition;
	let mesh: Mesh;

	const pointerSpring = spring($pointer);
	const cameraPositionSpring = spring(cameraOrigin);
	$: {
		pointerSpring.set($pointer);
	}
	$: {
		cameraPositionSpring.update(
			(c) => new Vector3(c.x, c.y + 8 * $pointerSpring.y, c.z - 8 * $pointerSpring.x)
		);
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
