<template>
  <div class="hero-canvas">
    <canvas ref="canvasRef" aria-hidden="true" />
  </div>
</template>

<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'

const canvasRef = ref<HTMLCanvasElement | null>(null)

let cleanup: (() => void) | null = null

const initThree = async () => {
  const canvas = canvasRef.value
  if (!canvas) {
    return
  }

  const module = await import(/* @vite-ignore */ 'https://cdn.skypack.dev/three@0.169.0?min')

  const {
    WebGLRenderer,
    Scene,
    PerspectiveCamera,
    AmbientLight,
    PointLight,
    Group,
    SphereGeometry,
    MeshStandardMaterial,
    Mesh,
    Color,
    CatmullRomCurve3,
    TubeGeometry,
    MeshBasicMaterial,
    BufferGeometry,
    Float32BufferAttribute,
    PointsMaterial,
    AdditiveBlending,
    Points,
    Vector2,
    Vector3,
    Clock,
    Material
  } = module

  const renderer = new WebGLRenderer({
    canvas,
    antialias: true,
    alpha: true,
    powerPreference: 'high-performance'
  })
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 1.75))

  const scene = new Scene()
  scene.background = null

  const camera = new PerspectiveCamera(45, 1, 0.1, 60)
  camera.position.set(0, 0.4, 8)

  const resize = () => {
    if (!canvas.parentElement) {
      return
    }

    const { clientWidth, clientHeight } = canvas.parentElement
    camera.aspect = clientWidth / clientHeight
    camera.updateProjectionMatrix()
    renderer.setSize(clientWidth, clientHeight, false)
  }

  resize()

  const ambient = new AmbientLight(0x7c85ff, 0.55)
  scene.add(ambient)

  const keyLight = new PointLight(0xff8dd6, 1.6, 25)
  keyLight.position.set(4, 3, 4)
  scene.add(keyLight)

  const backLight = new PointLight(0x6170ff, 1.2, 22)
  backLight.position.set(-5, -2, -4)
  scene.add(backLight)

  const orbs = new Group()
  scene.add(orbs)

  const orbGeometry = new SphereGeometry(0.22, 32, 32)
  const orbMaterial = new MeshStandardMaterial({
    color: new Color('#99a5ff'),
    emissive: new Color('#232450'),
    emissiveIntensity: 0.8,
    metalness: 0.65,
    roughness: 0.35
  })

  for (let i = 0; i < 5; i++) {
    const mesh = new Mesh(orbGeometry, orbMaterial.clone())
    const radius = 2.6 + i * 0.35
    const angle = (i / 5) * Math.PI * 2
    mesh.position.set(Math.cos(angle) * radius, Math.sin(angle * 1.2) * 0.9, Math.sin(angle) * radius)
    mesh.scale.setScalar(1 - i * 0.12)
    orbs.add(mesh)
  }

  const curvePoints: Vector3[] = []
  for (let i = 0; i <= 6; i++) {
    const angle = (i / 6) * Math.PI * 2
    curvePoints.push(new Vector3(Math.cos(angle) * 2.8, Math.sin(angle * 1.3) * 1.2, Math.sin(angle) * 2.8))
  }
  const curve = new CatmullRomCurve3(curvePoints, true)
  const curveGeometry = new TubeGeometry(curve, 200, 0.015, 12, true)
  const curveMaterial = new MeshBasicMaterial({ color: new Color('#4d5aff'), transparent: true, opacity: 0.55 })
  const curveMesh = new Mesh(curveGeometry, curveMaterial)
  scene.add(curveMesh)

  const particleGeometry = new BufferGeometry()
  const particleCount = 420
  const positions = new Float32Array(particleCount * 3)
  const basePositions = new Float32Array(particleCount * 3)
  const speeds: number[] = []
  for (let i = 0; i < particleCount; i++) {
    const theta = Math.random() * Math.PI * 2
    const phi = Math.acos(2 * Math.random() - 1)
    const r = 3.6 + Math.random() * 2.4
    const x = r * Math.sin(phi) * Math.cos(theta)
    const y = (Math.random() - 0.5) * 2.4
    const z = r * Math.sin(phi) * Math.sin(theta)
    const index = i * 3
    positions[index] = x
    positions[index + 1] = y
    positions[index + 2] = z
    basePositions[index] = x
    basePositions[index + 1] = y
    basePositions[index + 2] = z
    speeds.push(0.4 + Math.random() * 0.9)
  }
  particleGeometry.setAttribute('position', new Float32BufferAttribute(positions, 3))

  const particleMaterial = new PointsMaterial({
    size: 0.055,
    transparent: true,
    opacity: 0.65,
    blending: AdditiveBlending,
    color: new Color('#8fa3ff'),
    depthWrite: false
  })

  const particles = new Points(particleGeometry, particleMaterial)
  scene.add(particles)

  const pointer = new Vector2()
  const targetRotation = new Vector2(0.2, -0.15)

  const onPointerMove = (event: PointerEvent) => {
    const rect = canvas.getBoundingClientRect()
    pointer.x = (event.clientX - rect.left) / rect.width - 0.5
    pointer.y = (event.clientY - rect.top) / rect.height - 0.5
    targetRotation.x = pointer.x * 0.55
    targetRotation.y = pointer.y * -0.4
  }

  const clock = new Clock()

  let frameId = 0
  const animate = () => {
    frameId = requestAnimationFrame(animate)
    const elapsed = clock.getElapsedTime()

    orbs.rotation.y += (targetRotation.x - orbs.rotation.y) * 0.04
    orbs.rotation.x += (targetRotation.y - orbs.rotation.x) * 0.04

    curveMesh.rotation.y += 0.0025
    curveMesh.rotation.x += 0.0008

    const pos = particleGeometry.attributes.position as Float32BufferAttribute
    const arr = pos.array as Float32Array
    for (let i = 0; i < particleCount; i++) {
      const index = i * 3
      arr[index] = basePositions[index] + Math.sin(elapsed * speeds[i]) * 0.18
      arr[index + 1] = basePositions[index + 1] + Math.cos(elapsed * speeds[i] * 0.8) * 0.12
      arr[index + 2] = basePositions[index + 2] + Math.sin(elapsed * speeds[i] * 0.6) * 0.16
    }
    pos.needsUpdate = true

    renderer.render(scene, camera)
  }

  animate()

  window.addEventListener('resize', resize)
  canvas.addEventListener('pointermove', onPointerMove)

  cleanup = () => {
    cancelAnimationFrame(frameId)
    window.removeEventListener('resize', resize)
    canvas.removeEventListener('pointermove', onPointerMove)
    renderer.dispose()
    orbGeometry.dispose()
    orbs.traverse((child: any) => {
      if (child.isMesh) {
        const materials = Array.isArray(child.material) ? child.material : [child.material]
        materials.forEach((material: Material) => material.dispose())
      }
    })
    curveGeometry.dispose()
    curveMaterial.dispose()
    particleGeometry.dispose()
    particleMaterial.dispose()
  }
}

onMounted(() => {
  initThree()
})

onBeforeUnmount(() => {
  cleanup?.()
})
</script>
