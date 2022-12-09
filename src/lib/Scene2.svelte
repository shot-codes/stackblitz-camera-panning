<script lang="ts">
	import { T, TransformableObject, useFrame, useThrelte } from '@threlte/core';
	import { Mesh, Vector3 } from 'three';
	import { get } from 'svelte/store';

	const { pointer } = useThrelte();
	let cameraPosition = new Vector3(10, 10, 0);
	const cameraOrigin = cameraPosition;
	let mesh: Mesh;

	useFrame(({ camera }) => {
		get(camera).position.set(
			cameraOrigin.x,
			cameraOrigin.y + 8 * $pointer.y,
			cameraOrigin.z - 8 * $pointer.x
		);
	});
</script>

<T.PerspectiveCamera let:ref makeDefault fov={50}>
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
