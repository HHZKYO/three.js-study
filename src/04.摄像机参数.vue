<template></template>

<script setup>
import * as THREE from 'three';

// 目标：了解摄像机参数
// PerspectiveCamera：透视摄像机（模拟人眼感觉-近大远小），它会计算出一个视锥体的可视范围
// 参数1：垂直视野角度（0.1-179）
// 参数2：摄像机宽高比（和画布宽高比一致）等比例渲染画面
// 参数3：近截面距离
// 参数4：远截面距离
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );
const renderer = new THREE.WebGLRenderer();
renderer.setSize( window.innerWidth, window.innerHeight );
document.body.appendChild( renderer.domElement );

const geometry = new THREE.BoxGeometry( 1, 1, 1 );
// 注意：three.js里材质默认都是单面渲染（外面）
const material = new THREE.MeshBasicMaterial( { color: 0x00ff00 } );
const cube = new THREE.Mesh( geometry, material );
scene.add( cube );

// 默认摄像机物体和立方体都在世界的正中心
camera.position.z = 5;

renderer.render( scene, camera );
</script>

<style scoped></style>
