<script setup>
import * as THREE from 'three'
import { onMounted, ref } from 'vue'

const container1 = ref(null)
const container2 = ref(null)

onMounted(() => {
    // Scene 1 - Cube
    const scene = new THREE.Scene()
    const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
    const renderer = new THREE.WebGLRenderer({alpha: true, antialias: true})

    renderer.setSize(window.innerWidth, window.innerHeight)
    renderer.setPixelRatio(window.devicePixelRatio)

    container1.value.appendChild(renderer.domElement)

    const geometry = new THREE.BoxGeometry(2, 2, 2)
    const material = new THREE.MeshStandardMaterial({
        color: 0xff00ff,
        metalness: 0.1,
        roughness: 0.1
    })
    const cube = new THREE.Mesh(geometry, material)

    scene.add(cube)

    const ambientLight = new THREE.AmbientLight(0xffffff, 1)
    scene.add(ambientLight)

    const pointLight = new THREE.PointLight(0xffffff, 5)
    pointLight.position.set(5, 5, 5)
    scene.add(pointLight)

    camera.position.z = 5

    // Scene 2 - Pill
    const scene2 = new THREE.Scene()
    const camera2 = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
    const renderer2 = new THREE.WebGLRenderer({alpha: true, antialias: true})

    renderer2.setSize(window.innerWidth, window.innerHeight)
    renderer2.setPixelRatio(window.devicePixelRatio)

    renderer2.shadowMap.enabled = true
    renderer2.shadowMap.type = THREE.PCFShadowMap

    container2.value.appendChild(renderer2.domElement)

    const geometry2 = new THREE.CapsuleGeometry(1, 2, 64, 64)
    const material2 = new THREE.MeshStandardMaterial({
        color: 0xffffff,
        metalness: 0,
        roughness: 1
    })

    const pill = new THREE.Mesh(geometry2, material2)

    pill.receiveShadow = true

    scene2.add(pill)

    const ambientLight2 = new THREE.AmbientLight(0xffffff, 1)
    scene2.add(ambientLight2)

    const spotlight = new THREE.SpotLight(0xffffff, 100)
    spotlight.position.set(5, 5, 5)
    spotlight.castShadow = true
    spotlight.angle = Math.PI / 6
    scene2.add(spotlight)

    camera2.position.z = 5
   
    const raycaster = new THREE.Raycaster()
    const mouse = new THREE.Vector2()
    let mouseMoved = false

    function onMouseMove(event) {
        const rect = container2.value.getBoundingClientRect()
        mouse.x = ((event.clientX - rect.left) / rect.width) * 2 - 1
        mouse.y = - ((event.clientY - rect.top) / rect.height) * 2 + 1
        mouseMoved = true
    }

    container2.value.addEventListener('mousemove', onMouseMove)

    // Animation
    function animate() {
        cube.rotation.x += 0.01
        cube.rotation.y += 0.01

        pill.rotation.y += 0.01
        pill.rotation.z += 0.01

        if (mouseMoved) {
            raycaster.setFromCamera(mouse, camera2)
            const intersects = raycaster.intersectObject(pill)

            if (intersects.length > 0) {
                pill.material.color.set(0xf270c7)
            } else {
                pill.material.color.set(0xffffff)
                if (colorInterval) {
                    clearInterval(colorInterval)
                    colorInterval = null
                }
            }
        }

        renderer.render(scene, camera)
        renderer2.render(scene2, camera2)

        requestAnimationFrame(animate)
    }
    animate()
})
</script>

<template>
  <div ref="container1" class="three-container full-height"></div>
  <div ref="container2" class="three-container full-height"></div>
</template>

<style scoped>

</style>