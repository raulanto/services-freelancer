<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue'
import gsap from 'gsap'

const sectionRef = ref(null)
const titleRef = ref(null)
const activeCategory = ref(0)
let ctx: gsap.Context

const categories = [
  { id: 0, label: 'Frontend', desc: 'Experiencias reactivas y modernas' },
  { id: 1, label: 'Backend', desc: 'Motores robustos y seguros' },
  { id: 2, label: 'Datos & Servidores', desc: 'Infraestructura y datos avanzados' },
  { id: 3, label: 'Diseño & Flujo', desc: 'Del concepto al despliegue' },
]

const rows = [
  // Row 1 - Frontend (left to right)
  [
    { name: 'Vue', icon: 'https://skillicons.dev/icons?i=vue', cat: 0 },
    { name: 'Angular', icon: 'https://skillicons.dev/icons?i=angular', cat: 0 },
    { name: 'Nuxt', icon: 'https://skillicons.dev/icons?i=nuxt', cat: 0 },
    { name: 'Astro', icon: 'https://skillicons.dev/icons?i=astro', cat: 0 },
    { name: 'TypeScript', icon: 'https://skillicons.dev/icons?i=ts', cat: 0 },
    { name: 'JavaScript', icon: 'https://skillicons.dev/icons?i=js', cat: 0 },
    { name: 'Tailwind', icon: 'https://skillicons.dev/icons?i=tailwind', cat: 0 },
    { name: 'Pinia', icon: 'https://skillicons.dev/icons?i=pinia', cat: 3 },
  ],
  // Row 2 - Backend (right to left)
  [
    { name: '.NET', icon: 'https://skillicons.dev/icons?i=dotnet', cat: 1 },
    { name: 'Django', icon: 'https://skillicons.dev/icons?i=django', cat: 1 },
    { name: 'NestJS', icon: 'https://skillicons.dev/icons?i=nest', cat: 1 },
    { name: 'Laravel', icon: 'https://skillicons.dev/icons?i=laravel', cat: 1 },
    { name: 'Go', icon: 'https://skillicons.dev/icons?i=go', cat: 1 },
    { name: 'Node.js', icon: 'https://skillicons.dev/icons?i=nodejs', cat: 1 },
    { name: 'PHP', icon: 'https://skillicons.dev/icons?i=php', cat: 1 },
  ],
  // Row 3 - Data & Servers + Design (left to right)
  [
    { name: 'PostgreSQL', icon: 'https://skillicons.dev/icons?i=postgresql', cat: 2 },
    { name: 'MySQL', icon: 'https://skillicons.dev/icons?i=mysql', cat: 2 },
    { name: 'Docker', icon: 'https://skillicons.dev/icons?i=docker', cat: 2 },
    { name: 'Kubernetes', icon: 'https://skillicons.dev/icons?i=kubernetes', cat: 2 },
    { name: 'Figma', icon: 'https://skillicons.dev/icons?i=figma', cat: 3 },
    { name: 'Git', icon: 'https://skillicons.dev/icons?i=git', cat: 3 },
    { name: 'Linux', icon: 'https://skillicons.dev/icons?i=linux', cat: 2 },
  ],
]

// Duplicate each row for seamless infinite loop
const doubledRows = rows.map(r => [...r, ...r, ...r])

const hoveredTech = ref(null)

onMounted(() => {
  ctx = gsap.context(() => {
    gsap.from(titleRef.value, {
      y: 60, opacity: 0, duration: 1, ease: 'power3.out',
      scrollTrigger: { trigger: sectionRef.value, start: 'top 75%' }
    })
  }, sectionRef.value)
})

onUnmounted(() => ctx?.revert())
</script>

<template>
  <section ref="sectionRef" class="relative w-full py-32 bg-white dark:bg-neutral-950 overflow-hidden transition-colors duration-300">

    <!-- Dramatic background -->
    <div class="absolute inset-0 pointer-events-none">
      <div class="absolute inset-0 bg-[radial-gradient(ellipse_80%_50%_at_50%_50%,rgba(255,255,255,0.03)_0%,transparent_70%)]"></div>
      <!-- horizontal divider lines -->
      <div class="absolute inset-0 bg-[linear-gradient(transparent_calc(50%-1px),rgba(255,255,255,0.03)_50%,transparent_calc(50%+1px))]"></div>
    </div>

    <!-- Top fade -->
    <div class="absolute top-0 left-0 right-0 h-32 bg-gradient-to-b from-white dark:from-neutral-950 to-transparent z-20 pointer-events-none transition-colors duration-300"></div>
    <!-- Bottom fade -->
    <div class="absolute bottom-0 left-0 right-0 h-32 bg-gradient-to-t from-white dark:from-neutral-950 to-transparent z-20 pointer-events-none transition-colors duration-300"></div>
    <!-- Left fade -->
    <div class="absolute top-0 left-0 bottom-0 w-32 bg-gradient-to-r from-white dark:from-neutral-950 to-transparent z-20 pointer-events-none transition-colors duration-300"></div>
    <!-- Right fade -->
    <div class="absolute top-0 right-0 bottom-0 w-32 bg-gradient-to-l from-white dark:from-neutral-950 to-transparent z-20 pointer-events-none transition-colors duration-300"></div>

    <div class="relative z-10">
      <!-- Title -->
      <div ref="titleRef" class="max-w-6xl mx-auto px-4 mb-20 text-center">
        <p class="text-neutral-500 font-mono text-xs tracking-[0.25em] uppercase mb-4 transition-colors">Stack Tecnológico</p>
        <h2 class="text-4xl md:text-6xl font-serif text-neutral-900 dark:text-neutral-100 tracking-tight mb-6 transition-colors">
          El <span class="italic text-neutral-500 dark:text-neutral-400">arsenal</span>
        </h2>
        <p class="text-lg text-neutral-600 dark:text-neutral-400 max-w-xl mx-auto font-light leading-relaxed transition-colors">
          Cada herramienta fue elegida por su robustez y capacidad de escalar junto a tu negocio.
        </p>
      </div>

      <!-- Category filter pills -->
      <div class="flex justify-center gap-3 flex-wrap px-4 mb-16 relative z-30">
        <button
          v-for="cat in categories"
          :key="cat.id"
          @click="activeCategory = activeCategory === cat.id ? null : cat.id"
          class="relative px-4 py-2 rounded-full text-sm font-medium transition-all duration-300 border"
          :class="activeCategory === cat.id
            ? 'bg-neutral-800 dark:bg-neutral-100 text-white dark:text-neutral-900 border-neutral-800 dark:border-neutral-100 shadow-md dark:shadow-[0_0_20px_rgba(255,255,255,0.15)]'
            : 'bg-neutral-100 dark:bg-neutral-900/60 text-neutral-500 dark:text-neutral-400 border-neutral-200 dark:border-neutral-800 hover:border-neutral-300 dark:hover:border-neutral-600 hover:text-neutral-800 dark:hover:text-neutral-200'"
        >
          {{ cat.label }}
        </button>
      </div>

      <!-- Marquee rows -->
      <div class="space-y-6">
        <!-- Row 1: left → right -->
        <div class="relative overflow-hidden">
          <div class="flex gap-4 w-max animate-marquee-right hover:[animation-play-state:paused]">
            <div
              v-for="(tech, i) in doubledRows[0]"
              :key="`r0-${i}`"
              @mouseenter="hoveredTech = tech"
              @mouseleave="hoveredTech = null"
              class="group relative flex flex-col items-center justify-center w-24 h-24 rounded-2xl bg-neutral-50 dark:bg-neutral-900/60 border transition-all duration-500 cursor-pointer shrink-0"
              :class="[
                (activeCategory === null || activeCategory === tech.cat)
                  ? 'border-neutral-200 dark:border-neutral-800 opacity-100'
                  : 'border-neutral-100 dark:border-neutral-900 opacity-30 dark:opacity-20',
                hoveredTech?.name === tech.name
                  ? 'border-neutral-300 dark:border-neutral-500 bg-white dark:bg-neutral-800/80 scale-110 shadow-xl dark:shadow-[0_0_30px_rgba(255,255,255,0.1)] z-10'
                  : ''
              ]"
            >
              <img :src="tech.icon" :alt="tech.name" class="w-10 h-10 object-contain transition-transform duration-300 group-hover:scale-110" />
              <span class="mt-2 text-[10px] text-neutral-500 group-hover:text-neutral-300 transition-colors font-medium">{{ tech.name }}</span>
            </div>
          </div>
        </div>

        <!-- Row 2: right → left -->
        <div class="relative overflow-hidden">
          <div class="flex gap-4 w-max animate-marquee-left hover:[animation-play-state:paused]">
            <div
              v-for="(tech, i) in doubledRows[1]"
              :key="`r1-${i}`"
              @mouseenter="hoveredTech = tech"
              @mouseleave="hoveredTech = null"
              class="group relative flex flex-col items-center justify-center w-24 h-24 rounded-2xl bg-neutral-50 dark:bg-neutral-900/60 border transition-all duration-500 cursor-pointer shrink-0"
              :class="[
                (activeCategory === null || activeCategory === tech.cat)
                  ? 'border-neutral-200 dark:border-neutral-800 opacity-100'
                  : 'border-neutral-100 dark:border-neutral-900 opacity-30 dark:opacity-20',
                hoveredTech?.name === tech.name
                  ? 'border-neutral-300 dark:border-neutral-500 bg-white dark:bg-neutral-800/80 scale-110 shadow-xl dark:shadow-[0_0_30px_rgba(255,255,255,0.1)] z-10'
                  : ''
              ]"
            >
              <img :src="tech.icon" :alt="tech.name" class="w-10 h-10 object-contain transition-transform duration-300 group-hover:scale-110" />
              <span class="mt-2 text-[10px] text-neutral-500 group-hover:text-neutral-300 transition-colors font-medium">{{ tech.name }}</span>
            </div>
          </div>
        </div>

        <!-- Row 3: left → right (slower) -->
        <div class="relative overflow-hidden">
          <div class="flex gap-4 w-max animate-marquee-right-slow hover:[animation-play-state:paused]">
            <div
              v-for="(tech, i) in doubledRows[2]"
              :key="`r2-${i}`"
              @mouseenter="hoveredTech = tech"
              @mouseleave="hoveredTech = null"
              class="group relative flex flex-col items-center justify-center w-24 h-24 rounded-2xl bg-neutral-50 dark:bg-neutral-900/60 border transition-all duration-500 cursor-pointer shrink-0"
              :class="[
                (activeCategory === null || activeCategory === tech.cat)
                  ? 'border-neutral-200 dark:border-neutral-800 opacity-100'
                  : 'border-neutral-100 dark:border-neutral-900 opacity-30 dark:opacity-20',
                hoveredTech?.name === tech.name
                  ? 'border-neutral-300 dark:border-neutral-500 bg-white dark:bg-neutral-800/80 scale-110 shadow-xl dark:shadow-[0_0_30px_rgba(255,255,255,0.1)] z-10'
                  : ''
              ]"
            >
              <img :src="tech.icon" :alt="tech.name" class="w-10 h-10 object-contain transition-transform duration-300 group-hover:scale-110" />
              <span class="mt-2 text-[10px] text-neutral-500 group-hover:text-neutral-300 transition-colors font-medium">{{ tech.name }}</span>
            </div>
          </div>
        </div>
      </div>

      <Transition name="fade-up">
        <div v-if="hoveredTech" class="fixed bottom-12 left-1/2 -translate-x-1/2 z-50 pointer-events-none">
          <div class="flex items-center gap-4 bg-white/90 dark:bg-neutral-900/90 backdrop-blur-xl border border-neutral-200 dark:border-neutral-700 rounded-2xl px-6 py-4 shadow-2xl transition-colors duration-300">
            <img :src="hoveredTech.icon" class="w-10 h-10" />
            <div>
              <p class="text-neutral-900 dark:text-white font-semibold text-lg leading-none transition-colors">{{ hoveredTech.name }}</p>
              <p class="text-neutral-500 dark:text-neutral-400 text-sm mt-1 transition-colors">{{ categories[hoveredTech.cat]?.label }}</p>
            </div>
          </div>
        </div>
      </Transition>

    </div>
  </section>
</template>

<style scoped>
@keyframes marquee-right {
  0% { transform: translateX(0); }
  100% { transform: translateX(-33.333%); }
}
@keyframes marquee-left {
  0% { transform: translateX(-33.333%); }
  100% { transform: translateX(0); }
}

.animate-marquee-right {
  animation: marquee-right 30s linear infinite;
}
.animate-marquee-left {
  animation: marquee-left 25s linear infinite;
}
.animate-marquee-right-slow {
  animation: marquee-right 40s linear infinite;
}

.fade-up-enter-active,
.fade-up-leave-active {
  transition: all 0.25s cubic-bezier(0.34, 1.56, 0.64, 1);
}
.fade-up-enter-from,
.fade-up-leave-to {
  opacity: 0;
  transform: translateX(-50%) translateY(12px);
}
</style>
