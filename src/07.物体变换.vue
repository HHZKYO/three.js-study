<template></template>

<script setup>
import * as THREE from 'three';
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );
const renderer = new THREE.WebGLRenderer();
renderer.setSize( window.innerWidth, window.innerHeight );
document.body.appendChild( renderer.domElement );
const controls = new OrbitControls( camera, renderer.domElement );

// 目标：控制物体的变换
const geometry = new THREE.BoxGeometry( 1, 1, 1 );
const material = new THREE.MeshBasicMaterial( { color: 0x00ff00 } );
const cube = new THREE.Mesh( geometry, material );
scene.add( cube );

// 1.位移
// cube.position 值是一个 Vector3 的三维向量对象
cube.position.set(5, 0, 0)

// 3.缩放
cube.scale.set(2, 1, 1)

camera.position.set( 0, 2, 10 );

const axesHelper = new THREE.AxesHelper( 5 );
scene.add( axesHelper );

function animate() {
  requestAnimationFrame( animate );
  controls.update();
  renderer.render( scene, camera );
  // 2.旋转
  cube.rotation.x += 0.01
}
animate()
</script>

<style scoped></style>
