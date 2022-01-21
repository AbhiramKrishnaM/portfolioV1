<template>
  <div>
    <div class="container-1">
      <h1>Hello. <span>Iam Abhiram </span></h1>
    </div>
    <canvas class="webgl"></canvas>
    <section></section>
  </div>
</template>

<script>
import * as THREE from 'three'
// import * as dat from 'dat.gui'

import x from '../assets/NormalMap.png'

export default {
  name: 'IndexPage',

  mounted() {
    // loading

    const textureLoader = new THREE.TextureLoader()

    const normalTexture = textureLoader.load(x)

    // Debug
    // const gui = new dat.GUI()

    // Canvas
    const canvas = document.querySelector('canvas.webgl')

    // Scene
    const scene = new THREE.Scene()

    // Objects
    // const geometry = new THREE.TorusGeometry(0.7, 0.2, 16, 100)

    const geometry = new THREE.SphereGeometry(0.9, 24, 64)

    // Materials

    const material = new THREE.MeshStandardMaterial({
      metalness: 0.7,
      roughness: 0.2,
      normalMap: normalTexture,
    })

    material.color = new THREE.Color(0x292929)

    // Mesh
    const sphere = new THREE.Mesh(geometry, material)
    scene.add(sphere)

    // Lights

    const pointLight = new THREE.PointLight(0xffffff, 0.5)
    pointLight.position.x = 1
    pointLight.position.y = 2
    pointLight.position.z = 3
    scene.add(pointLight)

    // light 2
    const pointLight2 = new THREE.PointLight(0xff0000, 0.1)
    pointLight2.position.set(-0.28, 1.65, -2.04)
    pointLight2.intensity = 0.63
    scene.add(pointLight2)

    // light 3
    const pointLight3 = new THREE.PointLight(0x03dac6, 0.7)
    pointLight3.position.set(0.67, 0.87, -0.54)
    pointLight3.intensity = 0.95
    scene.add(pointLight3)

    // refernce our gui
    // const light3 = gui.addFolder('Light 3')
    // light3.add(pointLight3.position, 'x').min(-3).max(3).step(0.01)
    // light3.add(pointLight3.position, 'y').min(-6).max(6).step(0.01)
    // light3.add(pointLight3.position, 'z').min(-3).max(3).step(0.01)
    // light3.add(pointLight3, 'intensity').min(0).max(10).step(0.01)

    // const pointLightHelper = new THREE.PointLightHelper(pointLight3, 1)
    // scene.add(pointLightHelper)

    /**
     * Sizes
     */
    const sizes = {
      width: window.innerWidth,
      height: window.innerHeight,
    }

    window.addEventListener('resize', () => {
      // Update sizes
      sizes.width = window.innerWidth
      sizes.height = window.innerHeight

      // Update camera
      camera.aspect = sizes.width / sizes.height
      camera.updateProjectionMatrix()

      // Update renderer
      renderer.setSize(sizes.width, sizes.height)
      renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
    })

    /**
     * Camera
     */
    // Base camera
    const camera = new THREE.PerspectiveCamera(
      75,
      sizes.width / sizes.height,
      0.1,
      100
    )
    camera.position.x = 0
    camera.position.y = 0
    camera.position.z = 2
    scene.add(camera)

    // Controls
    // const controls = new OrbitControls(camera, canvas)
    // controls.enableDamping = true

    /**
     * Renderer
     */
    const renderer = new THREE.WebGLRenderer({
      canvas,
      alpha: true,
    })
    renderer.setSize(sizes.width, sizes.height)
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))

    /**
     * Animate
     */

    document.addEventListener('mousemove', onDocumentMouseMove)

    let mouseX = 0
    let mouseY = 0
    let targetX = 0
    let targetY = 0
    const windowHalfX = window.innerWidth / 2
    const windowHalfY = window.innerHeight / 2

    function onDocumentMouseMove(event) {
      mouseX = event.clientX - windowHalfX
      mouseY = event.clientY - windowHalfY
    }
    function updateSphere() {
      sphere.position.y = window.scrollY * 0.001
    }

    window.addEventListener('scroll', updateSphere)

    const tick = () => {
      targetX = mouseX * 0.001
      targetY = mouseY * 0.001

      // const elapsedTime = clock.getElapsedTime()

      // Update objects
      // sphere.rotation.x = -0.5 * elapsedTime

      sphere.rotation.y += 0.5 * (targetX - sphere.rotation.y)
      sphere.rotation.x += 0.05 * (targetY - sphere.rotation.x)
      sphere.position.z += -0.05 * (targetY - sphere.rotation.x)
      // Update Orbital Controls
      // controls.update()

      // Render
      renderer.render(scene, camera)

      // Call tick again on the next frame
      window.requestAnimationFrame(tick)
    }

    tick()
  },
}
</script>

<style>
.webgl {
  position: absolute;
  top: 0;
  left: 500px;
  outline: none;

  /* i dont know what this is */

  mix-blend-mode: exclusion;
}

.container-1 {
  height: 100vh;
  display: grid;
  place-items: center start;
}

h1 {
  font-size: 8rem;
  color: white;
}

span {
  font-size: 5rem;
  display: block;
}

section {
  height: 100vh;
}
</style>
