<template></template>

<script setup>
import * as THREE from 'three';
// 1.单独引入附加组件
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );
const renderer = new THREE.WebGLRenderer();
renderer.setSize( window.innerWidth, window.innerHeight );
document.body.appendChild( renderer.domElement );

// 2.实例化轨道控制器对象
// 参数1.摄像机对象；参数2. canvas 标签（内部回监听在 canvas 标签上的鼠标事件）
const controls = new OrbitControls( camera, renderer.domElement );

const geometry = new THREE.BoxGeometry( 1, 1, 1 );
const material = new THREE.MeshBasicMaterial( { color: 0x00ff00 } );
const cube = new THREE.Mesh( geometry, material );
scene.add( cube );

// 3.当修改了摄像机属性时，需调用轨道控制器 update 方法
// camera.position.z = 5;
camera.position.set( 0, 2, 10 );

const axesHelper = new THREE.AxesHelper( 5 );
scene.add( axesHelper );

// 4.引入渲染函数
function animate() {
  // requestAnimationFrame：在下一次重绘之前调用指定的函数体执行一次
  // 作用：更新动画的（不断的触发更新当下状态）
  // 为何要把 render 放在这里，因为我们需要让渲染器不断渲染最新的画面
  // 优点：浏览器切换到后台/标签页隐藏时会暂停调用
  requestAnimationFrame( animate );
  controls.update();
  renderer.render( scene, camera );
}
animate()
</script>

<style scoped></style>
