<script lang="ts">
    import {
        Canvas,
        Mesh,
        OrbitControls,
        PerspectiveCamera,
        HemisphereLight,
        Group,
    } from 'threlte';
    import { SVGLoader } from 'three/examples/jsm/loaders/SVGLoader.js';
    import beyonkSvg from './lib/beyonk.svg?raw';
    import upperSvg from './lib/upper.svg?raw';
    import lowerSvg from './lib/lower.svg?raw';
    import { onMount } from 'svelte';
    import * as THREE from 'three';

    let geometry: THREE.ExtrudeGeometry;
    let upperGeometry: THREE.ExtrudeGeometry;
    let lowerGeometry: THREE.ExtrudeGeometry;

    const geometryOptions = {
        curveSegments: 64,
        depth: 2,
        bevelEnabled: true,
        bevelOffset: 0,
        bevelSegments: 64,
        bevelSize: 1,
        bevelThickness: 2,
    };

    onMount(() => {
        const [upperShape] = new SVGLoader()
            .parse(upperSvg)
            .paths[0].toShapes(false);

        upperGeometry = new THREE.ExtrudeGeometry(upperShape, geometryOptions);
        upperGeometry.center();

        const [lowerShape] = new SVGLoader()
            .parse(lowerSvg)
            .paths[0].toShapes(false);

        lowerGeometry = new THREE.ExtrudeGeometry(lowerShape, geometryOptions);
        lowerGeometry.center();

        const shape = new SVGLoader().parse(beyonkSvg).paths[0].toShapes(true);

        geometry = new THREE.ExtrudeGeometry(shape, geometryOptions);
        geometry.center();
    });
</script>

<div>
    <Canvas>
        <PerspectiveCamera position={{ x: 0, y: 0, z: -200 }}>
            <OrbitControls autoRotate enableDamping />
        </PerspectiveCamera>

        <HemisphereLight
            skyColor={0x4c8eac}
            groundColor={0xac844c}
            intensity={1} />

        <Group position={{ x: 0, y: 25, z: 0 }}>
            {#if upperGeometry}
                <Mesh
                    castShadow
                    geometry={upperGeometry}
                    position={{ x: 0, y: 0, z: 0 }}
                    rotation={{ x: 0, y: 0, z: 0 }}
                    material={new THREE.MeshStandardMaterial({
                        color: new THREE.Color('#32B0A2'),
                        side: THREE.DoubleSide,
                    })} />
            {/if}

            {#if lowerGeometry}
                <Mesh
                    castShadow
                    geometry={lowerGeometry}
                    position={{ x: 0, y: 15, z: 0 }}
                    rotation={{ x: 0, y: 0, z: 0 }}
                    material={new THREE.MeshStandardMaterial({
                        color: new THREE.Color('#32B0A2'),
                        side: THREE.DoubleSide,
                    })} />
            {/if}
        </Group>

        {#if geometry}
            <Mesh
                castShadow
                {geometry}
                position={{ x: 0, y: 0, z: 0 }}
                rotation={{ x: Math.PI, y: Math.PI, z: 0 }}
                material={new THREE.MeshStandardMaterial({
                    color: new THREE.Color('#32B0A2'),
                    side: THREE.DoubleSide,
                })} />
        {/if}
    </Canvas>
</div>

<style>
    div {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: #222;
    }
</style>
