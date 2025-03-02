<template></template>

<script setup>
import * as THREE from 'three';

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );
const renderer = new THREE.WebGLRenderer();
renderer.setSize( window.innerWidth, window.innerHeight );
document.body.appendChild( renderer.domElement );

// 目标：添加一个立方体
// BoxGeometry 立方缓冲集合体（宽度、高度、深度）
// three.js 单位（米/厘米）
const geometry = new THREE.BoxGeometry( 1, 1, 1 );
// MeshBasicMaterial 基础网络材质（简单平面着色）
const material = new THREE.MeshBasicMaterial( { color: 0x00ff00 } );
// Mesh：网格物体（三角形围出来的平面）
const cube = new THREE.Mesh( geometry, material );
// 把立方体加入场景中
scene.add( cube );

camera.position.z = 5;

renderer.render( scene, camera );
</script>

<style scoped></style>
