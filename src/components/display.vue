<template>
  <div>
    <canvas ref="canvasRef"></canvas>
    <div class="overlay">
      <div id="mySidenav" class="sidenav" :style="{ width: sidenavWidth }">
        <a href="javascript:void(0)" class="closebtn" @click="closeNav">&times;</a>
        <a href="#" @click="loadTexture('https://mtcopeland.com/wp-content/uploads/2022/02/shutterstock_299819663-min-scaled.jpg')">Red</a>
        <a href="#" @click="loadTexture('https://images.unsplash.com/photo-1588345921523-c2dcdb7f1dcd?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8Mnx8d2hpdGV8ZW58MHx8MHx8fDA%3D&w=1000&q=80')">Texture 2</a>
        <a href="#" @click="loadTexture('https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQOz4Hn0YJPHFPVr2oiLfW8wA5uGkCD6YZmTw')">Texture 3</a>
      </div>
      <span class="opncls" :class="{ opened: sidenavWidth !== '0px' }" @click="toggleNav">&#9776;</span>
    </div>
  </div>
</template>

<script>
import * as THREE from 'three';
import { onMounted, ref } from "vue";

export default {
  setup() {
    const canvasRef = ref(null);
    let renderer;
    let scene = new THREE.Scene();

    const sidenavWidth = ref("0px");
    const selectedTexture = ref(null);
    const rerenderWithNewTexture = () => {
      // Load a new texture URL here
      const newTextureURL = 'https://images.unsplash.com/photo-1588345921523-c2dcdb7f1dcd?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8Mnx8d2hpdGV8ZW58MHx8MHx8fDA%3D&w=1000&q=80';
      defTexture(newTextureURL);
      updateWallTextures1();
    };
    const toggleNav = () => {
      if (sidenavWidth.value === "0px") {
        openNav();
      } else {
        closeNav();
      }
    };

    const openNav = () => {
      sidenavWidth.value = "250px";
    };

    const closeNav = () => {
      sidenavWidth.value = "0px";
    };


    let wallNow = null;

    const loadTexture = (url) => {
      const textureLoader = new THREE.TextureLoader();
      textureLoader.load(url, (texture) => {
        selectedTexture.value = texture;
        texture.wrapS = THREE.RepeatWrapping;
        texture.wrapT = THREE.RepeatWrapping;
        texture.repeat.set(4, 5);

        // Set the material map for the selected wall with the loaded texture
        if (wallNow) {
          wallNow.material.map = texture;
          wallNow.material.needsUpdate = true;

          // Render the scene with the updated texture
          renderer.render(scene, camera);
        }
      });
};

const defTexture = (url) => {
      const textureLoader = new THREE.TextureLoader();
      textureLoader.load(url, (texture) => {
        selectedTexture.value = texture;
          renderer.render(scene, camera);
        
      });
};

    let leftWall;
    let floorWall;
    let backWall;
    let rightWall;

    const createWalls = () => {
      // Create the left wall
      const leftGeometry = new THREE.PlaneGeometry(15, 37);
      const leftMaterial = new THREE.MeshStandardMaterial();
      leftWall = new THREE.Mesh(leftGeometry, leftMaterial);
      leftWall.rotateY(1.7);
      leftWall.position.set(-29, 3, -15);
      scene.add(leftWall);

      // Create the back wall
      const backGeometry = new THREE.PlaneGeometry(50, 32);
      const backMaterial = new THREE.MeshStandardMaterial();
      backWall = new THREE.Mesh(backGeometry, backMaterial);
      backWall.position.set(0.7, 2, -17);
      scene.add(backWall);

      // Create the right wall
      const rightGeometry = new THREE.PlaneGeometry(15, 38);
      const rightMaterial = new THREE.MeshStandardMaterial();
      rightWall = new THREE.Mesh(rightGeometry, rightMaterial);
      rightWall.position.set(29, 3, -15);
      rightWall.rotateY(-1.7);
      scene.add(rightWall);

      // Create the floor
      const floorMaterial = new THREE.MeshStandardMaterial({ side: THREE.DoubleSide });
      const floorGeometry = new THREE.PlaneGeometry(27, 15);
      floorWall = new THREE.Mesh(floorGeometry, floorMaterial);
      floorWall.position.set(2, -7, 2.89);
      floorWall.rotateX(Math.PI / 2);
      scene.add(floorWall);

    };

    const setupScene = () => {
      createWalls();

      let ambientLight = new THREE.AmbientLight("#f0f2f0", 1);
      scene.add(ambientLight);

      let camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 100);
      camera.position.set(4, 0, 10);
      scene.add(camera);

      renderer = new THREE.WebGLRenderer({
        canvas: canvasRef.value,
        antialias: true,
      });
      renderer.setSize(window.innerWidth, window.innerHeight);
      renderer.setPixelRatio(window.devicePixelRatio);
      renderer.render(scene, camera);

      var wallNow=null


      function updateWallTextures1() {
        const clickedObject = this;
        const texture = selectedTexture.value;
          wallNow=leftWall
          openNav();
          leftWall.material.map = texture;
          leftWall.material.needsUpdate = true;
          wallNow=floorWall;
          openNav();
          floorWall.material.map = texture;
          floorWall.material.needsUpdate = true;
          wallNow=backWall
          openNav();
          backWall.material.map = texture;
          backWall.material.needsUpdate = true;
          wallNow=rightWall
          openNav();
          rightWall.material.map = texture;
          rightWall.material.needsUpdate = true;
        renderer.render(scene, camera); 
      }

      function updateWallTextures() {
        const clickedObject = this;
        const texture = selectedTexture.value;

        if (clickedObject === leftWall) {
          
          wallNow=leftWall
          openNav();
          leftWall.material.map = texture;
          leftWall.material.needsUpdate = true;
        } else if (clickedObject === floorWall) {

          wallNow=floorWall;
          openNav();
          floorWall.material.map = texture;
          floorWall.material.needsUpdate = true;
        } else if (clickedObject === backWall) {

          wallNow=backWall
          openNav();
          backWall.material.map = texture;
          backWall.material.needsUpdate = true;

        } else if (clickedObject === rightWall) {

          wallNow=rightWall
          openNav();
          rightWall.material.map = texture;
          rightWall.material.needsUpdate = true;

        }

        renderer.render(scene, camera); 
      }

      leftWall.onClick = updateWallTextures;
      floorWall.onClick = updateWallTextures;
      backWall.onClick = updateWallTextures;
      rightWall.onClick = updateWallTextures;

      window.addEventListener('click', onMouseClick, false);

      function onMouseClick(event) {
        const mouse = new THREE.Vector2(
          (event.clientX / window.innerWidth) * 2 - 1,
          -(event.clientY / window.innerHeight) * 2 + 1
        );

        const raycaster = new THREE.Raycaster();
        raycaster.setFromCamera(mouse, camera);

        const intersects = raycaster.intersectObjects([leftWall, backWall, rightWall, floorWall]);

        if (intersects.length > 0) {
          const clickedObject = intersects[0].object;

          if (clickedObject.onClick) {
            clickedObject.onClick();
          }
        }
      }
    };

    onMounted(() => {
      setupScene();
      rerenderWithNewTexture();
      updateWallTextures1();
    });

    return { canvasRef, sidenavWidth, toggleNav, closeNav, selectedTexture, loadTexture };
  },
};
</script>

<style scoped>
.overlay {
  position: absolute;
  top: 50px;
  left: 50px;
  z-index: 1;
  background-color: none;
  padding: 10px;
}

.sidenav {
  height: 100%;
  width: 0;
  position: fixed;
  z-index: 1;
  top: 0;
  left: 0;
  background-color: #111;
  overflow-x: hidden;
  transition: 0.5s;
  padding-top: 60px;
}

.sidenav a {
  padding: 8px 8px 8px 32px;
  text-decoration: none;
  font-size: 25px;
  color: #818181;
  display: block;
  transition: 0.3s;
}

.sidenav a:hover {
  color: #f1f1f1;
}

.sidenav .closebtn {
  position: absolute;
  top: 0;
  right: 25px;
  font-size: 36px;
}

.opncls {
  position: relative;
  display: inline-block;
  font-size: 30px;
  cursor: pointer;
}

.opened {
  margin-left: 250px;
}
</style>
