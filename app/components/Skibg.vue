<template>
    <div class="particle-container">
        <div
            v-for="particle in particles"
            :key="particle.id"
            class="particle"
            :style="{
        left: particle.x + '%',
        top: particle.y + '%',
        width: particle.size + 'px',
        height: particle.size + 'px',
        opacity: particle.opacity,
        transform: `rotate(${particle.rotation}deg)`
      }"
        >
            <div
                class="particle-tail"
                :style="{
          height: (particle.size * 3) + 'px'
        }"
            ></div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { shallowRef, triggerRef, onMounted, onUnmounted } from 'vue'

interface Particle {
    id: number
    x: number
    y: number
    size: number
    speed: number
    opacity: number
    rotation: number
}

const particles = shallowRef<Particle[]>([])
const animationId = shallowRef<number | null>(null)

const PARTICLE_COUNT = 15
const PARTICLE_SIZE = { min: 2, max: 2 }
const FALL_SPEED = { min: 0.5, max: 1 }

const createParticle = (): Particle => {
    return {
        id: Math.random(),
        x: Math.random() * 100,
        y: 110, // Start below the container
        size: Math.random() * (PARTICLE_SIZE.max - PARTICLE_SIZE.min) + PARTICLE_SIZE.min,
        speed: Math.random() * (FALL_SPEED.max - FALL_SPEED.min) + FALL_SPEED.min,
        opacity: Math.random() * 0.4 + 0.1, // More subtle
        rotation: Math.random() * 360
    }
}

const initParticles = (): void => {
    const initialParticles = []
    for (let i = 0; i < PARTICLE_COUNT; i++) {
        const particle = createParticle()
        particle.y = Math.random() * 110 // Random initial Y position
        initialParticles.push(particle)
    }
    particles.value = initialParticles
}

const animate = (): void => {
    const currentParticles = particles.value
    for (let i = 0; i < currentParticles.length; i++) {
        const particle = currentParticles[i]
        particle.y -= particle.speed // Move upwards
        particle.rotation += 2

        if (particle.y < -10) {
            currentParticles[i] = createParticle()
        }
    }
    triggerRef(particles) // Trigger update since we use shallowRef

    animationId.value = requestAnimationFrame(animate)
}

onMounted(() => {
    initParticles()
    animate()
})

onUnmounted(() => {
    if (animationId.value) {
        cancelAnimationFrame(animationId.value)
    }
})
</script>

<style scoped>
.particle-container {
    position: absolute; /* Only inside the parent section */
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    overflow: hidden;
}

.particle {
    position: absolute;
    border-radius: 50%;
    background: linear-gradient(135deg, #e5e5e5, #a3a3a3, #737373); /* Neutral colors */
    box-shadow:
        0 0 10px rgba(163, 163, 163, 0.4),
        0 0 20px rgba(163, 163, 163, 0.2);
    animation: twinkle 2s infinite alternate;
    transition: transform 0.1s;
    will-change: top, left, transform; /* Performance optimization */
}

.particle-tail {
    position: absolute;
    bottom: -16px; /* Tail goes downwards */
    left: 50%;
    width: 2px;
    opacity: 0.4;
    background: linear-gradient(to bottom, #a3a3a3, transparent); /* Neutral tail */
    transform: translateX(-50%);
}

.particle::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    border-radius: 50%;
    background: radial-gradient(circle, rgba(163, 163, 163, 0.6) 0%, transparent 70%); /* Neutral glow */
    animation: pulse 1.5s infinite;
}

@keyframes twinkle {
    0% {
        filter: brightness(1);
    }
    100% {
        filter: brightness(1.3);
    }
}

@keyframes pulse {
    0%, 100% {
        transform: scale(1);
        opacity: 0.6;
    }
    50% {
        transform: scale(1.2);
        opacity: 0.3;
    }
}
</style>