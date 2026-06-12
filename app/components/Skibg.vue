<template>
    <div class="particle-container">
        <div
            v-for="particle in particles"
            :key="particle.id"
            class="particle"
            :class="[colorTheme]"
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
                :class="[colorTheme]"
                :style="{ height: (particle.size * 4) + 'px' }"
            ></div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { shallowRef, triggerRef, onMounted, onUnmounted, ref } from 'vue'

const props = defineProps({
  colorTheme: {
    type: String,
    default: 'neutral' // 'neutral', 'saas', 'custom'
  }
})

interface Particle {
    id: number
    x: number
    y: number
    vx: number
    vy: number
    size: number
    speed: number
    opacity: number
    rotation: number
    baseX: number
}

const particles = shallowRef<Particle[]>([])
const animationId = shallowRef<number | null>(null)

// Increase presence: more particles, bigger size, faster
const PARTICLE_COUNT = 35
const PARTICLE_SIZE = { min: 3, max: 7 }
const FALL_SPEED = { min: 1.0, max: 2.0 }

// Mouse interaction tracking
const mouseX = ref(-100)
const mouseY = ref(-100)

const onMouseMove = (e: MouseEvent) => {
    mouseX.value = (e.clientX / window.innerWidth) * 100
    mouseY.value = (e.clientY / window.innerHeight) * 100
}

const createParticle = (): Particle => {
    const x = Math.random() * 100
    return {
        id: Math.random(),
        x: x,
        baseX: x,
        y: 110, // Start below the container
        vx: 0,
        vy: 0,
        size: Math.random() * (PARTICLE_SIZE.max - PARTICLE_SIZE.min) + PARTICLE_SIZE.min,
        speed: Math.random() * (FALL_SPEED.max - FALL_SPEED.min) + FALL_SPEED.min,
        opacity: Math.random() * 0.5 + 0.3, // Brighter
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
        
        // Base upward movement
        particle.y -= particle.speed
        particle.rotation += 1.5

        // Mouse interaction (Repulsion effect)
        const dx = particle.x - mouseX.value
        const dy = particle.y - mouseY.value
        const distance = Math.sqrt(dx * dx + dy * dy)
        
        // Repel if mouse is close (distance is in % relative to screen)
        if (distance < 12) {
            const force = (12 - distance) / 12
            particle.vx += (dx / distance) * force * 0.8
            particle.vy += (dy / distance) * force * 0.8
        }
        
        // Apply velocity and damping (friction)
        particle.x += particle.vx
        particle.y += particle.vy
        particle.vx *= 0.85
        particle.vy *= 0.85
        
        // Gently drift back to baseX to avoid clumping
        particle.x += (particle.baseX - particle.x) * 0.02

        // Reset particle if it goes off screen
        if (particle.y < -10 || particle.x < -10 || particle.x > 110) {
            currentParticles[i] = createParticle()
        }
    }
    triggerRef(particles)

    animationId.value = requestAnimationFrame(animate)
}

onMounted(() => {
    window.addEventListener('mousemove', onMouseMove)
    initParticles()
    animate()
})

onUnmounted(() => {
    window.removeEventListener('mousemove', onMouseMove)
    if (animationId.value) {
        cancelAnimationFrame(animationId.value)
    }
})
</script>

<style scoped>
.particle-container {
    position: absolute;
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
    animation: twinkle 2s infinite alternate;
    transition: background 0.8s ease, box-shadow 0.8s ease;
    will-change: top, left, transform;
}

/* Neutral Theme */
.particle.neutral {
    background: linear-gradient(135deg, #e5e5e5, #a3a3a3);
    box-shadow: 0 0 15px rgba(163, 163, 163, 0.6);
}
.particle-tail.neutral {
    background: linear-gradient(to bottom, #a3a3a3, transparent);
}

/* SaaS Theme (Red) */
.particle.saas {
    background: linear-gradient(135deg, #fca5a5, #ef4444);
    box-shadow: 0 0 25px rgba(239, 68, 68, 0.9);
}
.particle-tail.saas {
    background: linear-gradient(to bottom, #ef4444, transparent);
}

/* Custom Theme (Blue) */
.particle.custom {
    background: linear-gradient(135deg, #93c5fd, #3b82f6);
    box-shadow: 0 0 25px rgba(59, 130, 246, 0.9);
}
.particle-tail.custom {
    background: linear-gradient(to bottom, #3b82f6, transparent);
}

.particle-tail {
    position: absolute;
    bottom: -8px;
    left: 50%;
    width: 2px;
    opacity: 0.6;
    transform: translateX(-50%);
    transition: background 0.8s ease;
}

.particle::before {
    content: '';
    position: absolute;
    top: -50%;
    left: -50%;
    right: -50%;
    bottom: -50%;
    border-radius: 50%;
    animation: pulse 2s infinite;
    transition: background 0.8s ease;
}

.particle.neutral::before { background: radial-gradient(circle, rgba(163, 163, 163, 0.4) 0%, transparent 70%); }
.particle.saas::before { background: radial-gradient(circle, rgba(239, 68, 68, 0.4) 0%, transparent 70%); }
.particle.custom::before { background: radial-gradient(circle, rgba(59, 130, 246, 0.4) 0%, transparent 70%); }

@keyframes twinkle {
    0% { filter: brightness(1); }
    100% { filter: brightness(1.5); }
}

@keyframes pulse {
    0%, 100% { transform: scale(1); opacity: 0.8; }
    50% { transform: scale(1.5); opacity: 0.3; }
}
</style>