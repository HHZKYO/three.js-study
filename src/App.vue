<template></template>

<script setup>
import * as THREE from 'three'; 
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
import gsap from 'gsap';
// 目标：把glb模型文件加载到网页中
// 1. 引入 GLTFLoader 加载器
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";

// 2. 实例化加载器对象
const gltfLoader = new GLTFLoader();

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
camera.position.set(5, 2, 3)
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
    const car3d = gltf.scene
    scene.add(car3d);

    // 问题1：模型黑色看不到，模型本身没有自发光，我们需要添加一个环境光照亮所有受到光照影响的物体
    // 环境光
    const light = new THREE.AmbientLight(0xffffff, 2);
    scene.add(light);
    // 创建光线投射对象
    const raycaster = new THREE.Raycaster();
    // 初始化二维坐标点（空）
    const pointer = new THREE.Vector2();
    // 目标：交互绑定
    // Empty001_16 -> Object_64
    const leftDoor = car3d.getObjectByName("Empty001_16");
    // Empty002_20 -> Object_77
    const rightDoor = car3d.getObjectByName("Empty002_20");
    function onPointerMove( event ) {
      pointer.x = ( event.clientX / window.innerWidth ) * 2 - 1;
      pointer.y = - ( event.clientY / window.innerHeight ) * 2 + 1;
      raycaster.setFromCamera( pointer, camera );
      const intersects = raycaster.intersectObjects( car3d.children );
      console.log(intersects);
      if (intersects.length > 0) {
        const target = intersects[0].object
        console.log(target)
        if (target.name === 'Object_64') {
          // 左车门，车门原点不在车角落处，角落是另一个父级的父级组物体的原点
          // userData.自定义属性
          if (!target.userData.isOpen) {
            // 打开
            gsap.to(leftDoor.rotation, {
              x: Math.PI / 4,
              duration: 2,
            })
          } else {
            // 关闭
            gsap.to(leftDoor.rotation, {
              x: 0,
              duration: 2,
            })
          }
          target.userData.isOpen = !target.userData.isOpen
        }
      }
    }
    window.addEventListener( 'click', onPointerMove );
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

// 设置地面
// 加载图片得到纹理对象
const floorTexture = new THREE.TextureLoader().load(new URL('@/assets/sand.jpg', import.meta.url).href)
floorTexture.colorSpace = THREE.SRGBColorSpace;
const geometry = new THREE.CircleGeometry( 10, 32 );
const material = new THREE.MeshBasicMaterial( { map: floorTexture } );
const circle = new THREE.Mesh( geometry, material );
scene.add( circle );
circle.rotation.set(-Math.PI / 2, 0, 0)

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
