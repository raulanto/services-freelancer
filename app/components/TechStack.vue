<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue'
import gsap from 'gsap'

const sectionRef = ref(null)
const titleRef = ref(null)
const groupsRef = ref([])
let ctx: gsap.Context

const groups = [
  {
    label: 'Frontend',
    tagline: 'Experiencias de usuario reactivas y modernas',
    techs: [
      { name: 'Vue', icon: 'https://skillicons.dev/icons?i=vue' },
      { name: 'Angular', icon: 'https://skillicons.dev/icons?i=angular' },
      { name: 'Nuxt', icon: 'https://skillicons.dev/icons?i=nuxt' },
      { name: 'Astro', icon: 'https://skillicons.dev/icons?i=astro' },
      { name: 'TypeScript', icon: 'https://skillicons.dev/icons?i=ts' },
      { name: 'JavaScript', icon: 'https://skillicons.dev/icons?i=js' },
    ]
  },
  {
    label: 'Backend & Arquitectura',
    tagline: 'Motores robustos, seguros y escalables',
    techs: [
      { name: '.NET', icon: 'https://skillicons.dev/icons?i=dotnet' },
      { name: 'Django', icon: 'https://skillicons.dev/icons?i=django' },
      { name: 'NestJS', icon: 'https://skillicons.dev/icons?i=nest' },
      { name: 'Laravel', icon: 'https://skillicons.dev/icons?i=laravel' },
      { name: 'Go', icon: 'https://skillicons.dev/icons?i=go' },
    ]
  },
  {
    label: 'Bases de Datos & Servidores',
    tagline: 'Manejo avanzado de datos y automatización de entornos',
    techs: [
      { name: 'PostgreSQL', icon: 'https://skillicons.dev/icons?i=postgresql' },
      { name: 'MySQL', icon: 'https://skillicons.dev/icons?i=mysql' },
      { name: 'Docker', icon: 'https://skillicons.dev/icons?i=docker' },
      { name: 'Kubernetes', icon: 'https://skillicons.dev/icons?i=kubernetes' },
    ]
  },
  {
    label: 'Diseño & Flujo',
    tagline: 'Procesos limpios desde la idea hasta el despliegue',
    techs: [
      { name: 'Figma', icon: 'https://skillicons.dev/icons?i=figma' },
      { name: 'Git', icon: 'https://skillicons.dev/icons?i=git' },
      { name: 'Pinia', icon: 'https://skillicons.dev/icons?i=pinia' },
    ]
  }
]

onMounted(() => {
  ctx = gsap.context(() => {
    gsap.from(titleRef.value, {
      y: 60,
      opacity: 0,
      duration: 1,
      ease: 'power3.out',
      scrollTrigger: {
        trigger: sectionRef.value,
        start: 'top 75%',
        toggleActions: 'play none none reverse'
      }
    })

    groupsRef.value.forEach((group, i) => {
      gsap.from(group, {
        y: 80,
        opacity: 0,
        duration: 0.8,
        delay: i * 0.1,
        ease: 'power3.out',
        scrollTrigger: {
          trigger: group,
          start: 'top 82%',
          toggleActions: 'play none none reverse'
        }
      })

      // Stagger each icon chip inside the group
      const chips = group.querySelectorAll('.tech-chip')
      gsap.from(chips, {
        scale: 0.6,
        opacity: 0,
        duration: 0.5,
        stagger: 0.07,
        ease: 'back.out(1.7)',
        scrollTrigger: {
          trigger: group,
          start: 'top 80%',
          toggleActions: 'play none none reverse'
        }
      })
    })
  }, sectionRef.value)
})

onUnmounted(() => {
  ctx?.revert()
})
</script>

<template>
  <section ref="sectionRef" class="relative w-full py-32 bg-neutral-950 overflow-hidden">
    <!-- Decorative ambient blobs -->
    <div class="absolute inset-0 pointer-events-none overflow-hidden">
      <div class="absolute -left-40 top-20 w-[500px] h-[500px] bg-neutral-800/10 rounded-full blur-[120px]"></div>
      <div class="absolute -right-40 bottom-20 w-[500px] h-[500px] bg-neutral-700/10 rounded-full blur-[120px]"></div>
    </div>

    <div class="max-w-6xl mx-auto px-4 relative z-10">
      <!-- Header -->
      <div ref="titleRef" class="mb-24">
        <p class="text-neutral-500 font-mono text-sm tracking-widest uppercase mb-4">Stack Tecnológico</p>
        <h2 class="text-4xl md:text-6xl font-serif text-neutral-100 tracking-tight">
          El <span class="italic text-neutral-400">arsenal</span>
        </h2>
        <p class="text-lg md:text-xl text-neutral-400 max-w-2xl mt-6 font-light leading-relaxed">
          Las herramientas adecuadas marcan la diferencia. Cada tecnología de este arsenal fue elegida por su robustez, rendimiento y capacidad de crecer junto a tu negocio.
        </p>
      </div>

      <!-- Groups Grid -->
      <div class="grid md:grid-cols-2 gap-6">
        <div
          v-for="(group, gi) in groups"
          :key="gi"
          :ref="el => groupsRef.push(el)"
          class="relative rounded-[2rem] border border-neutral-800/80 bg-neutral-900/30 backdrop-blur-md p-8 group overflow-hidden transition-all duration-500 hover:border-neutral-700 hover:bg-neutral-900/50 hover:shadow-[0_20px_60px_-20px_rgba(0,0,0,0.8)]"
        >
          <!-- Hover glow -->
          <div class="absolute inset-0 bg-gradient-to-br from-white/[0.02] to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-700 pointer-events-none rounded-[2rem]"></div>

          <!-- Group header -->
          <div class="mb-8 relative z-10">
            <h3 class="text-xl md:text-2xl font-medium text-white mb-2">{{ group.label }}</h3>
            <p class="text-sm text-neutral-500 font-light">{{ group.tagline }}</p>
          </div>

          <!-- Tech chips -->
          <div class="flex flex-wrap gap-3 relative z-10">
            <div
              v-for="tech in group.techs"
              :key="tech.name"
              class="tech-chip flex items-center gap-2.5 bg-neutral-900/80 border border-neutral-800 rounded-xl px-3 py-2 transition-all duration-300 hover:border-neutral-600 hover:bg-neutral-800/80 hover:-translate-y-1 hover:shadow-lg cursor-default"
            >
              <img
                :src="tech.icon"
                :alt="tech.name"
                class="w-6 h-6 object-contain shrink-0"
                loading="lazy"
              />
              <span class="text-sm font-medium text-neutral-300 whitespace-nowrap">{{ tech.name }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
