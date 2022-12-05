<template>
  <!-- <div class="home">
    <div class="canvas-cotainer" ref="canvasDom">
    </div>
  </div> -->

  <div class="content">
    <div class="three_bg" ref="threeBgDom">
    </div>
    <header>
      <div class="nav">
          <div class="nav_list">
               <li><a href="#"><span>Home</span></a></li>
               <li><a href="#"><span>About Us</span></a></li>
               <li><a href="#"><span>3D background</span></a></li>
               <li><a href="#"><span>Projects</span></a></li>
               <li><a href="#"><span>Contact</span></a></li>
          </div>
      </div>
    </header>
    <div class="description">
        <h1>Hello</h1>
        <h2>This is a <br> 3D background</h2>
        <h2>Enjoy</h2>
    </div>
  </div>
  <!-- <HelloWorld msg="Vite + Vue" /> -->
</template>

<script setup>
 import * as THREE from "three"
 import { onMounted, ref } from "vue";
 import bg1 from "./assets/marcin-jozwiak-Jmsw8wYy7Ig-unsplash.jpg"
import bg2 from "./assets/michael-olsen--djS1aPrSr4-unsplash.jpg"

 let   scene, camera, renderer, geometry, material, mesh, container, count;

 let threeBgDom = ref(null)

 const clock = new THREE.Clock();


 onMounted(()=>{
    // 实例化场景
    scene = new THREE.Scene()

    // 实例化相机
    camera = new THREE.PerspectiveCamera(70, window.innerWidth/window.innerHeight, 0.1, 1000)
    camera.position.z = 5;

    // 材质
    const loader = new THREE.TextureLoader()
    geometry = new THREE.PlaneGeometry(17, 8, 15, 9);
    material = new THREE.MeshBasicMaterial({
       map: loader.load(bg2)
    })
    mesh = new THREE.Mesh(geometry, material)
    scene.add(mesh)

    // 渲染器
    renderer = new THREE.WebGL1Renderer();
    renderer.setSize(window.innerWidth, window.innerHeight);
    container = threeBgDom.value.appendChild(renderer.domElement)
    
    count = geometry.attributes.position.count; 
    animate()
    // setTimeout(()=>{
    //   animate()
    // }, 3000)
    // renderer.render(scene, camera)
 })
 function animate() {
    const time = clock.getElapsedTime();
    for(let i = 0; i < count; i++){
      const x = geometry.attributes.position.getX(i)
      const y = geometry.attributes.position.getY(i)

      const animi1 = 0.25 * Math.sin(x + time * 0.7)
      const animi2 = 0.35 * Math.sin(x * 1 + time * 0.7)
      const animi3 = 0.1 * Math.sin(y * 15 + time * 0.7)

      // geometry.attributes.position.setZ(i, animi1);
      // geometry.attributes.position.setZ(i, animi1 + animi2);
      geometry.attributes.position.setZ(i, animi1 + animi2 + animi3);
      geometry.computeVertexNormals();
      geometry.attributes.position.needsUpdate = true
    }
    // console.log(1)
    requestAnimationFrame(animate)
    renderer.render(scene, camera)
  }
  
 
//  var oldTime = 0;


</script>

<style scoped>
.three_bg {
   position: absolute;
   width: 100%;
   height: 100vh;
}
</style>
