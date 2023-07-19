# react-three-fiber-ornforlag-concept-viz

for lulz. webmaster's year 2020 publishing brand workshop kick of

(from webmastha to b.o.t, peace! )

 Edit index.js
```
import ReactDOM from 'react-dom'
import * as THREE from 'three'
import React, { Suspense, useEffect, useRef, useState } from 'react'
import { Canvas, useLoader, useFrame } from 'react-three-fiber'
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader'
import Text from './Text'
import './styles.css'

// ----------------------|BELOW IS THE RENDERD TEXT, MUST BE CAPS, NO SPECIAL CHARS!|------------

function Jumbo() {
  const ref = useRef()
  useFrame(({ clock }) => (ref.current.rotation.x = ref.current.rotation.y = ref.current.rotation.z = Math.sin(clock.getElapsedTime()) * 0.3))
  return (
    <group ref={ref}>
      <Text hAlign="left" position={[0, 4.2, 0]} children="ORN" />
      <Text hAlign="left" position={[0, 0, 0]} children="FORLAG" />
      <Text hAlign="left" position={[0, -4.2, 0]} children="VARMOGNATURLIG" size={0.5} />
      <Text hAlign="left" position={[12, 0, 0]} children="4" size={3} />
      <Text hAlign="left" position={[16.5, -4.2, 0]} children="U" />
    </group>
  )
}

// This component was auto-generated from GLTF by: https://github.com/react-spring/gltfjsx
// ------------BIRDS ORBETING ------- 
//THREEJS EXAMPLE MATERIAL BIRDS REUSED

function Bird({ speed, factor, url, ...props }) {
  const gltf = useLoader(GLTFLoader, url)
  const group = useRef()
  const [mixer] = useState(() => new THREE.AnimationMixer())
  //THIS LINE BELOW MAKES THEM FLAP THEIR WINGS
  useEffect(() => void mixer.clipAction(gltf.animations[0], group.current).play(), [])
  //SO THEY ALL KNOW THEIR DIRECTION AND CENTER 
  useFrame((state, delta) => {
    group.current.rotation.y += Math.sin((delta * factor) / 2) * Math.cos((delta * factor) / 2) * 1.5
    mixer.update(delta * speed)
  })
  return (
    <group ref={group}>
      <scene name="Scene" {...props}>
        <mesh
          name="Object_0"
          morphTargetDictionary={gltf.__$[1].morphTargetDictionary}
          morphTargetInfluences={gltf.__$[1].morphTargetInfluences}
          rotation={[1.5707964611537577, 0, 0]}>
          <bufferGeometry attach="geometry" {...gltf.__$[1].geometry} />
          <meshStandardMaterial attach="material" {...gltf.__$[1].material} name="Material_0_COLOR_0" />
        </mesh>
      </scene>
    </group>
  )
}

function Birds() {
//CONTROLS WHERE BIRDS ARE SPAWND AND THEIR SPEED, DIRECTION AND SPATIAL MOTION RELATIVE TO THEM SELF
  return new Array(100).fill().map((_, i) => {
    const x = (15 + Math.random() * 30) * (Math.round(Math.random()) ? -1 : 1)
    const y = -10 + Math.random() * 20
    const z = -5 + Math.random() * 10
    const bird = ['Stork', 'Parrot', 'Flamingo'][Math.round(Math.random() * 2)]
    let speed = bird === 'Stork' ? 0.5 : bird === 'Flamingo' ? 2 : 5
    let factor = bird === 'Stork' ? 0.5 + Math.random() : bird === 'Flamingo' ? 0.25 + Math.random() : 1 + Math.random() - 0.5
    return <Bird key={i} position={[x, y, z]} rotation={[0, x > 0 ? Math.PI : 0, 0]} speed={speed} factor={factor} url={`/${bird}.glb`} />
  })
}
// THREEJS (CANVAS, LIGHT, CAMERA) THIS CONTROLS THE VIEWPORT

function App() {
  return (
    <Canvas camera={{ position: [0, 0, 46] }}>
      <ambientLight intensity={2} />
      <pointLight position={[40, 40, 40]} />
      <Suspense fallback={null}>
        <Jumbo />
        <Birds />
      </Suspense>
    </Canvas>
  )
}

ReactDOM.render(<App />, document.getElementById('root'))
