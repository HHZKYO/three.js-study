<template></template>

<script setup>
import * as THREE from 'three'; 
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
camera.position.z = 5
const renderer = new THREE.WebGLRenderer()
renderer.setSize(window.innerWidth, window.innerHeight)
document.body.appendChild(renderer.domElement)

const axesHelper = new THREE.AxesHelper( 5 )
scene.add( axesHelper )

const controls = new OrbitControls(camera, renderer.domElement)

function renderLoop() {
  renderer.render(scene, camera)
  controls.update()
  requestAnimationFrame(renderLoop)
}
renderLoop()


window.addEventListener('resize', () => {
  renderer.setSize(window.innerWidth, window.innerHeight)
  camera.aspect = window.innerWidth / window.innerHeight
  camera.updateProjectionMatrix()
})
</script>

<style scoped></style>
