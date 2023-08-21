<template>
  <canvas ref="canvasRef">
 
  </canvas>

</template>

<script>
import * as THREE from 'three';

import { onMounted, ref } from "vue";

export default {
  setup() {
    const canvasRef = ref(null);
    let renderer;
    let scene = new THREE.Scene();

    const material = new THREE.MeshStandardMaterial({ color: 0x808080 });
    const leftGeometry = new THREE.PlaneGeometry(15, 37);
    const leftwall = new THREE.Mesh(leftGeometry, material);
    
    leftwall.position.set(10, 30, -15);
    scene.add(leftwall);

    let ambientLight = new THREE.AmbientLight("#f0f2f0", 1);
    scene.add(ambientLight);

    let camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 100);
    camera.position.set(4 ,0,10);
    scene.add(camera);

    onMounted(() => {
      renderer = new THREE.WebGLRenderer({
        canvas: canvasRef.value,
        antialias: true,
        alpha: true,
      });
      renderer.setSize(window.innerWidth, window.innerHeight);
      renderer.setPixelRatio(window.devicePixelRatio);
      renderer.render(scene, camera);
    });

    return { canvasRef };
  }
};
</script>

<style scoped>
</style>
