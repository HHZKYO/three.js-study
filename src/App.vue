<template></template>

<script setup>
import * as THREE from 'three'; // 步骤1.三要素
import { OrbitControls } from 'three/addons/controls/OrbitControls.js'; // 步骤3.引入轨道控制器
const scene = new THREE.Scene(); // 步骤1.三要素
const camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 ); // 步骤1.三要素
camera.position.set(0, 0, 50) // 步骤5.设置相机位置
const renderer = new THREE.WebGLRenderer(); // 步骤1.三要素
renderer.setSize( window.innerWidth, window.innerHeight ); // 步骤1.三要素
document.body.appendChild( renderer.domElement ); // 步骤1.三要素
const controls = new OrbitControls( camera, renderer.domElement ); // 步骤3

// 步骤4.放入物体
const geometry = new THREE.SphereGeometry( 15, 32, 16 ); // 步骤4.放入物体
const material = new THREE.MeshBasicMaterial( { color: 0x0000ff } ); // 步骤4.放入物体
const sphere = new THREE.Mesh( geometry, material ); // 步骤4.放入物体
scene.add( sphere );

// 步骤7.环境贴图，添加场景的背景
// CubeTextureLoader：立方体纹理加载器（专门用来加载 6 张图片包裹盒的）
const cubeTL = new THREE.CubeTextureLoader()
// 调用 load 方法，按 x 正负，y 正负，z 正负的顺序传入相应的图片
const cubeTexture = cubeTL.load([
  // vite 构建工具：js 中加载静态资源要使用这种格式：new URL('静态资源路径'，import.meta.url).href
  new URL('@/assets/sky/1.jpg', import.meta.url).href,
  new URL('@/assets/sky/2.jpg', import.meta.url).href,
  new URL('@/assets/sky/3.jpg', import.meta.url).href,
  new URL('@/assets/sky/4.jpg', import.meta.url).href,
  new URL('@/assets/sky/5.jpg', import.meta.url).href,
  new URL('@/assets/sky/6.jpg', import.meta.url).href,
])
scene.background = cubeTexture

// 步骤8.光线投射 —> 鼠标拾取
// 创建光线投射对象
const raycaster = new THREE.Raycaster();
// 初始化二维坐标点（空）
const pointer = new THREE.Vector2();
// 点击事件处理函数
function onPointerMove( event ) {
  // 将鼠标位置归一化为设备坐标。x 和 y 方向的取值范围是 (-1 to +1)
  pointer.x = ( event.clientX / window.innerWidth ) * 2 - 1;
  pointer.y = - ( event.clientY / window.innerHeight ) * 2 + 1;
  // 通过摄像机和鼠标位置更新射线
  raycaster.setFromCamera( pointer, camera );
  // 计算物体和射线的焦点
  const intersects = raycaster.intersectObjects( scene.children );
  for ( let i = 0; i < intersects.length; i ++ ) {
    intersects[ i ].object.material.color.set( 0xff0000 );
  }
}
window.addEventListener( 'click', onPointerMove );

// 步骤6.适配
window.addEventListener('resize', () => {
  // 画布的最新宽高
  renderer.setSize(window.innerWidth, window.innerHeight);
  // 画布的最新宽高比
  camera.aspect = window.innerWidth / window.innerHeight;
  // 强制更新摄像机的计算矩阵
  camera.updateProjectionMatrix()
})

// 步骤2.准备渲染循环的递归函数
function animate() {
  renderer.render( scene, camera ); // 步骤2.准备渲染循环的递归函数
  controls.update(); // 步骤3
  requestAnimationFrame( animate ); // 步骤2.准备渲染循环的递归函数
}
animate()
</script>

<style scoped></style>
