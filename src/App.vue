<script setup lang="ts">
import * as THREE from 'three'
import { FontLoader } from 'three/addons/loaders/FontLoader.js';
import { TextGeometry } from 'three/addons/geometries/TextGeometry.js';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
import TWEEN from '@tweenjs/tween.js';
import { onMounted } from 'vue';
const canvasWH = {
  width: 1800,
  height: 1000
}
const camera = new THREE.PerspectiveCamera(
  50,
  canvasWH.width / canvasWH.height,
  0.1,
  3000
)

const scene = new THREE.Scene()
scene.background = new THREE.Color('#ffffff')
camera.position.set(500, canvasWH.width, canvasWH.height);

const hemiLight = new THREE.HemisphereLight(0xffffff, 0x999999);
hemiLight.position.set(0, 20, 0);
scene.add(hemiLight);

onMounted(() => {
  const canvas = document.getElementById("three") as HTMLElement

  const renderer = new THREE.WebGLRenderer({ canvas, antialias: true })
  renderer.setSize(canvasWH.width, canvasWH.height);

  const controls = new OrbitControls(camera, renderer.domElement)
  controls.enableDamping = true
  controls.enablePan = false
  controls.enableZoom = false;
  controls.minPolarAngle = controls.maxPolarAngle = Math.PI * (90 / 180);

  const loader = new FontLoader();
  loader.load('fonts/gentilis_bold.typeface.json', function (font) {
    const geometry = new TextGeometry('D', {
      font: font,
      size: 520,
      height: 150,
      curveSegments: 12,
      bevelEnabled: true,
      bevelThickness: 10,
      bevelSize: 0,
      bevelOffset: 0,
      bevelSegments: 20
    });
    const materials = [
      new THREE.MeshPhongMaterial({ color: 0x000000, flatShading: true }), // front
      new THREE.MeshPhongMaterial({ color: 0xffffff }) // side
    ];
    const textMesh1 = new THREE.Mesh(geometry, materials);
    scene.add(textMesh1)

    const geometry2 = new TextGeometry('X', {
      font: font,
      size: 520,
      height: 150,
      curveSegments: 12,
      bevelEnabled: true,
      bevelThickness: 10,
      bevelSize: 0,
      bevelOffset: 0,
      bevelSegments: 20
    });
    const textMesh2 = new THREE.Mesh(geometry2, materials);
    textMesh2.position.x = 520
    scene.add(textMesh2)
  });

  const render = () => {
    renderer.render(scene, camera)
    controls.update();
    requestAnimationFrame(render);
  };
  render()
})


const onScorll = () => {
  const tWEEN = new TWEEN.Tween(camera.position)
  tWEEN.to({ x: 300, y: 300, z: 300 }, 5000)
  tWEEN.start().easing(TWEEN.Easing.Quadratic.Out);
  console.log(tWEEN)
}
</script>

<template>
  <div>
    <canvas id="three" :width="canvasWH.width" :height="canvasWH.height"></canvas>
    <button @click="onScorll">111</button>
  </div>
</template>

<style scoped></style>
