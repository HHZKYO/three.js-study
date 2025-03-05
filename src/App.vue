<template></template>

<script setup>
import * as THREE from 'three'; 
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
// 目标：把glb模型文件加载到网页中
// 1. 引入 GLTFLoader 加载器
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";

// 2. 实例化加载器对象
const gltfLoader = new GLTFLoader();

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
camera.position.z = 5
const renderer = new THREE.WebGLRenderer()
renderer.setSize(window.innerWidth, window.innerHeight)
document.body.appendChild(renderer.domElement)

const axesHelper = new THREE.AxesHelper( 5 )
scene.add( axesHelper )

const controls = new OrbitControls(camera, renderer.domElement)

// 3. 加载 glb 模型对象
gltfLoader.load(
  new URL("@/assets/glb/lbgn.glb", import.meta.url).href,
  (gltf) => {
    // 4. gltf.scene 才能拿到模型数据对象，添加到场景中
    scene.add(gltf.scene);

    // 问题1：模型黑色看不到，模型本身没有自发光，我们需要添加一个环境光照亮所有受到光照影响的物体
    // 环境光
    const light = new THREE.AmbientLight(0xffffff, 2);
    scene.add(light);
  }
);

// 环境贴图
// const cubeTL = new THREE.CubeTextureLoader()
// const cubeTexture = cubeTL.load([
//   new URL('@/assets/sky/1.jpg', import.meta.url).href,
//   new URL('@/assets/sky/2.jpg', import.meta.url).href,
//   new URL('@/assets/sky/3.jpg', import.meta.url).href,
//   new URL('@/assets/sky/4.jpg', import.meta.url).href,
//   new URL('@/assets/sky/5.jpg', import.meta.url).href,
//   new URL('@/assets/sky/6.jpg', import.meta.url).href,
// ])
// scene.background = cubeTexture
const texture = new THREE.TextureLoader().load(
  new URL("@/assets/desert.jpg", import.meta.url).href
);
// 设置图片对比度和环绕方式
texture.colorSpace = THREE.SRGBColorSpace;
texture.mapping = THREE.EquirectangularReflectionMapping;
scene.background = texture;

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
