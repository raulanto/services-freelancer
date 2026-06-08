<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import gsap from 'gsap'
import ScrollTrigger from 'gsap/ScrollTrigger'
import { ArrowRight, Users } from '@lucide/vue'
import Skibg from '~/components/Skibg.vue'

gsap.registerPlugin(ScrollTrigger)

const containerRef = ref<HTMLElement | null>(null)
const orbitalRef = ref<HTMLElement | null>(null)
const avatarsRef = ref<HTMLElement[]>([])
let ctx: gsap.Context

// ===========================================
// CÁLCULO TRIGONOMÉTRICO EXACTO
// El contenedor es 480x480px → radio del contenedor = 240px (centro = 240, 240)
// Los anillos están definidos como % del 90% del contenedor (432px):
//   Exterior: r = 216px (50% de 432)
//   Medio:    r = 151px (35% de 432)
//   Interior: r = 86px  (20% de 432)
// Posición = center + r * cos/sin(angle) con center = 216 (50% de 432)
// ===========================================

const SIZE = 432 // w-[90%] de 480px
const CENTER = SIZE / 2 // 216

function polarToPercent(radiusPct: number, angleDeg: number) {
  const r = SIZE * radiusPct
  const rad = (angleDeg - 90) * (Math.PI / 180) // -90 para empezar desde arriba
  const x = CENTER + r * Math.cos(rad)
  const y = CENTER + r * Math.sin(rad)
  return {
    left: `${(x / SIZE) * 100}%`,
    top:  `${(y / SIZE) * 100}%`,
  }
}

// Seeds variados para avatares únicos
const seeds = ['alpha', 'beta', 'gamma', 'delta', 'epsilon', 'zeta', 'eta', 'theta', 'iota', 'kappa', 'lambda', 'mu']

const orbitalAvatars = [
  // Órbita exterior (r = 48%)
  { ...polarToPercent(0.48,   0), size: 40, img: `https://api.dicebear.com/7.x/avataaars/svg?seed=${seeds[0]}` },
  { ...polarToPercent(0.48,  72), size: 36, img: `https://api.dicebear.com/7.x/avataaars/svg?seed=${seeds[1]}` },
  { ...polarToPercent(0.48, 144), size: 44, img: `https://api.dicebear.com/7.x/avataaars/svg?seed=${seeds[2]}` },
  { ...polarToPercent(0.48, 216), size: 40, img: `https://api.dicebear.com/7.x/avataaars/svg?seed=${seeds[3]}` },
  { ...polarToPercent(0.48, 288), size: 36, img: `https://api.dicebear.com/7.x/avataaars/svg?seed=${seeds[4]}` },

  // Órbita media (r = 32%)
  { ...polarToPercent(0.32,  45), size: 40, img: `https://api.dicebear.com/7.x/avataaars/svg?seed=${seeds[5]}` },
  { ...polarToPercent(0.32, 135), size: 32, img: `https://api.dicebear.com/7.x/avataaars/svg?seed=${seeds[6]}` },
  { ...polarToPercent(0.32, 225), size: 44, img: `https://api.dicebear.com/7.x/avataaars/svg?seed=${seeds[7]}` },
  { ...polarToPercent(0.32, 315), size: 32, img: `https://api.dicebear.com/7.x/avataaars/svg?seed=${seeds[8]}` },

  // Órbita interior (r = 18%)
  { ...polarToPercent(0.18,  90), size: 44, img: `https://api.dicebear.com/7.x/avataaars/svg?seed=${seeds[9]}` },
  { ...polarToPercent(0.18, 210), size: 36, img: `https://api.dicebear.com/7.x/avataaars/svg?seed=${seeds[10]}` },
  { ...polarToPercent(0.18, 330), size: 32, img: `https://api.dicebear.com/7.x/avataaars/svg?seed=${seeds[11]}` },
]

const stats = [
  { value: '15+', label: 'Proyectos' },
  { value: '100%', label: 'A Medida' },
  { value: '24/7', label: 'Soporte' },
]

onMounted(() => {
  ctx = gsap.context(() => {
    // Entrada del sistema orbital completo
    gsap.from(orbitalRef.value, {
      scale: 0.85,
      opacity: 0,
      duration: 1.2,
      ease: 'power3.out',
      scrollTrigger: {
        trigger: containerRef.value,
        start: 'top 80%',
      }
    })

    // Pop-in de los avatares con stagger
    gsap.from(avatarsRef.value, {
      scale: 0,
      opacity: 0,
      duration: 0.5,
      stagger: 0.04,
      ease: 'back.out(2)',
      scrollTrigger: {
        trigger: containerRef.value,
        start: 'top 70%',
      }
    })

    // Rotación orbital lenta e infinita del contenedor principal
    gsap.to(orbitalRef.value, {
      rotation: 360,
      duration: 100,
      repeat: -1,
      ease: 'none'
    })

    // Contra-rotación de cada avatar para que siempre miren "de frente"
    gsap.to(avatarsRef.value, {
      rotation: -360,
      duration: 100,
      repeat: -1,
      ease: 'none'
    })

  }, containerRef.value!)
})

onUnmounted(() => ctx?.revert())
</script>

<template>
  <div ref="containerRef" class="grid lg:grid-cols-2 gap-16 lg:gap-8 items-center mt-20">

    <!-- ─── LADO IZQUIERDO: Textos y estadísticas ─── -->
    <div class="order-2 lg:order-1 text-center lg:text-left">
      
      <p class="inline-flex items-center gap-2 text-xs font-mono tracking-[0.2em] uppercase text-zinc-500 mb-6">
        <Users class="w-3.5 h-3.5" />
        Comunidad
      </p>

      <h3 class="text-4xl md:text-5xl font-serif text-zinc-900 dark:text-white mb-6 tracking-tight transition-colors leading-tight">
        La diferencia<br />
        <span class="italic text-zinc-400 dark:text-zinc-500">de ser</span>
      </h3>

      <p class="text-lg text-zinc-500 dark:text-zinc-400 mb-12 leading-relaxed max-w-md mx-auto lg:mx-0 font-light transition-colors">
        Trabajamos junto a diseñadores, desarrolladores expertos y empresas excepcionales para construir el mejor ecosistema tecnológico.
      </p>
      
      <!-- Estadísticas -->
      <div class="flex items-center justify-center lg:justify-start gap-10 mb-12">
        <div v-for="stat in stats" :key="stat.label">
          <div class="text-3xl md:text-4xl font-semibold text-zinc-900 dark:text-white mb-1 transition-colors tabular-nums">
            {{ stat.value }}
          </div>
          <div class="text-zinc-500 text-xs font-medium uppercase tracking-widest transition-colors">
            {{ stat.label }}
          </div>
        </div>
      </div>

      <button class="group inline-flex items-center gap-2 px-6 py-3 rounded-full bg-zinc-900 dark:bg-white text-white dark:text-zinc-900 text-sm font-medium hover:bg-zinc-700 dark:hover:bg-zinc-100 transition-all duration-300 shadow-lg">
        Conocer al equipo
        <ArrowRight class="w-4 h-4 transition-transform duration-300 group-hover:translate-x-1" />
      </button>
    </div>

    <!-- ─── LADO DERECHO: Sistema Orbital ─── -->
    <div class="order-1 lg:order-2 relative flex items-center justify-center">

      <!-- Contenedor cuadrado fijo -->
      <div class="relative w-[480px] max-w-full aspect-square">

        <!-- Fondo Skibg difuminado -->
        <div class="absolute inset-0 z-0 opacity-15 dark:opacity-30 mask-radial pointer-events-none">
          <ClientOnly>
            <Skibg />
          </ClientOnly>
        </div>

        <!-- Sistema que rota (Anillos + avatares juntos) -->
        <div
          ref="orbitalRef"
          class="absolute inset-[5%] z-10"
          style="transform-origin: center center;"
        >
          <!-- ── Anillos SVG (perfectamente centrados) ── -->
          <svg
            class="absolute inset-0 w-full h-full"
            viewBox="0 0 432 432"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <!-- Órbita exterior -->
            <circle
              cx="216" cy="216"
              :r="432 * 0.48"
              class="stroke-zinc-200 dark:stroke-zinc-800 transition-colors"
              stroke-width="1"
              stroke-dasharray="4 8"
            />
            <!-- Órbita media -->
            <circle
              cx="216" cy="216"
              :r="432 * 0.32"
              class="stroke-zinc-300 dark:stroke-zinc-700 transition-colors"
              stroke-width="1"
            />
            <!-- Órbita interior -->
            <circle
              cx="216" cy="216"
              :r="432 * 0.18"
              class="stroke-zinc-200 dark:stroke-zinc-800 transition-colors"
              stroke-width="1"
              stroke-dasharray="2 6"
            />
          </svg>

          <!-- ── Avatares posicionados exactamente sobre las órbitas ── -->
          <div
            v-for="(avatar, i) in orbitalAvatars"
            :key="i"
            :ref="(el) => avatarsRef.push(el as HTMLElement)"
            class="absolute group cursor-pointer"
            :style="{
              left: avatar.left,
              top: avatar.top,
              width: `${avatar.size}px`,
              height: `${avatar.size}px`,
              marginLeft: `-${avatar.size / 2}px`,
              marginTop: `-${avatar.size / 2}px`,
            }"
            style="transform-origin: center center;"
          >
            <!-- Glow en hover -->
            <div class="absolute inset-0 rounded-full bg-blue-400/20 dark:bg-blue-400/30 scale-0 group-hover:scale-150 transition-transform duration-500 blur-sm"></div>
            <!-- Avatar imagen -->
            <div class="relative w-full h-full rounded-full overflow-hidden border-2 border-white/80 dark:border-zinc-700 bg-white dark:bg-zinc-900 shadow-lg dark:shadow-xl group-hover:border-blue-400/60 group-hover:shadow-blue-400/20 group-hover:shadow-xl transition-all duration-300 z-10">
              <img
                :src="avatar.img"
                :alt="`Colaborador ${i + 1}`"
                class="w-full h-full object-cover scale-110 opacity-80 group-hover:opacity-100 transition-opacity duration-300"
                loading="lazy"
              />
            </div>
          </div>
        </div>

        <!-- ── Avatar central (fuera del sistema que rota) ── -->
        <div class="absolute inset-0 flex items-center justify-center z-20 pointer-events-none">
          <div class="relative w-20 h-20 rounded-full overflow-hidden border-2 border-zinc-200 dark:border-zinc-600 bg-white dark:bg-zinc-950 shadow-2xl dark:shadow-[0_0_40px_rgba(59,130,246,0.2)] ring-4 ring-white/50 dark:ring-zinc-800/50">
            <img
              src="https://api.dicebear.com/7.x/avataaars/svg?seed=Raul&backgroundColor=27272a"
              alt="Avatar principal"
              class="w-full h-full object-cover scale-110"
            />
          </div>
        </div>

      </div><!-- /contenedor cuadrado -->
    </div><!-- /lado derecho -->

  </div>
</template>

<style scoped>
.mask-radial {
  mask-image: radial-gradient(circle at center, black 20%, transparent 75%);
  -webkit-mask-image: radial-gradient(circle at center, black 20%, transparent 75%);
}
</style>
