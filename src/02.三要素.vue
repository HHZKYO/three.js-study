<template></template>

<script setup>
// 1.引入 three.js
// * as 收集所有 export 命名导出的属性和方法报保存到 THREE 变量中
import * as THREE from 'three';

// 2.要素1：创建场景（容器/世界）
const scene = new THREE.Scene();
//3.要素2：创建摄像机
const camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );

// 4.要素3：创建渲染器
const renderer = new THREE.WebGLRenderer();
// 给渲染器设置画布宽高
renderer.setSize( window.innerWidth, window.innerHeight );
// renderer.domElement: canvas 画布标签添加到真实 DOM 上，才能看到
// canvas标签（画布标签）：可以用代码决定绘制的画面效果
document.body.appendChild( renderer.domElement );
// 把场景和摄像机传递给渲染器，开始在 canvas 画布上渲染计算后的画面
renderer.render( scene, camera );

</script>

<style scoped></style>
