<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import gsap from 'gsap'
import ScrollTrigger from 'gsap/ScrollTrigger'
import { Target, Lightbulb } from '@lucide/vue'
import OrbitalCommunity from '~/components/OrbitalCommunity.vue'

gsap.registerPlugin(ScrollTrigger)

const containerRef = ref<HTMLElement | null>(null)
const headerRef = ref<HTMLElement | null>(null)
const missionSectionRef = ref<HTMLElement | null>(null)
const orbitalSectionRef = ref<HTMLElement | null>(null)
const cardsRef = ref<HTMLElement[]>([])

const setCardRef = (el: any) => {
  if (el && !cardsRef.value.includes(el)) {
    cardsRef.value.push(el)
  }
}

let ctx: gsap.Context

onMounted(() => {
  ctx = gsap.context(() => {
    
    // Animación del Encabezado
    gsap.from(headerRef.value, {
      y: 100,
      opacity: 0,
      duration: 1,
      ease: 'power3.out',
      scrollTrigger: {
        trigger: headerRef.value,
        start: 'top 85%',
        toggleActions: 'play none none reverse'
      }
    })

    // Animación de los bloques de Misión/Visión
    cardsRef.value.forEach((item) => {
      gsap.from(item, {
        y: 100,
        opacity: 0,
        duration: 1,
        ease: 'power3.out',
        scrollTrigger: {
          trigger: item,
          start: 'top 85%',
          toggleActions: 'play none none reverse'
        }
      })
    })

    // Animación de la sección Orbital
    gsap.from(orbitalSectionRef.value, {
      opacity: 0,
      y: 100,
      duration: 1,
      ease: 'power3.out',
      scrollTrigger: {
        trigger: orbitalSectionRef.value,
        start: 'top 85%',
        toggleActions: 'play none none reverse'
      }
    })

  }, containerRef.value!)
})

onUnmounted(() => {
  ctx?.revert()
})
</script>

<template>
  <div ref="containerRef" class="relative w-full bg-neutral-50 dark:bg-[#0a0a0a] overflow-hidden transition-colors duration-300">
    <!-- Glow radial de fondo -->
    <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[1000px] h-[1000px] bg-zinc-200/40 dark:bg-zinc-800/20 rounded-full blur-[150px] pointer-events-none transition-colors z-0"></div>

    <!-- 1. Encabezado y Misión/Visión (Pantalla completa unificada) -->
    <section class="relative z-10 min-h-[100dvh] flex flex-col items-center justify-center px-6 lg:px-8 max-w-7xl mx-auto py-24 gap-12 lg:gap-16">
      <div ref="headerRef" class="text-center w-full max-w-4xl pt-10 md:pt-0">
        <div class="inline-flex items-center gap-2 px-4 py-2 mb-6 rounded-full bg-zinc-200/80 dark:bg-zinc-900/80 border border-zinc-300 dark:border-zinc-800 text-sm font-medium text-zinc-700 dark:text-zinc-300 tracking-wide transition-colors backdrop-blur-sm">
          <span class="w-2.5 h-2.5 rounded-full bg-blue-500 shadow-[0_0_12px_rgba(59,130,246,0.8)]"></span>
          Sobre Nosotros
        </div>
        <h2 class="text-5xl md:text-6xl lg:text-7xl font-serif text-zinc-900 dark:text-zinc-100 mb-6 tracking-tight transition-colors">
          Quiénes <span class="italic text-zinc-500">Somos</span>
        </h2>
        <p class="text-lg md:text-xl lg:text-2xl text-zinc-600 dark:text-zinc-400 mx-auto font-light leading-relaxed transition-colors">
          No somos una agencia tradicional que entrega código y desaparece. Somos aliados tecnológicos comprometidos con el éxito y la escalabilidad de tu negocio.
        </p>
      </div>

      <div ref="missionSectionRef" class="grid md:grid-cols-2 gap-6 lg:gap-10 w-full pb-10 md:pb-0">
        <!-- Misión -->
        <div :ref="setCardRef" class="mission-card bg-gradient-to-br from-blue-50/80 to-white/60 dark:from-blue-950/30 dark:to-zinc-900/40 backdrop-blur-xl border border-blue-100/80 dark:border-blue-900/50 rounded-[2.5rem] p-8 lg:p-12 transition-all duration-700 hover:shadow-xl hover:shadow-blue-500/10 hover:border-blue-300/80 dark:hover:border-blue-700/60 hover:-translate-y-2 shadow-2xl shadow-blue-900/5 dark:shadow-none group">
          <div class="w-16 h-16 bg-blue-100/60 dark:bg-blue-900/40 border border-blue-200 dark:border-blue-800/60 rounded-2xl flex items-center justify-center mb-8 shadow-lg shadow-blue-500/5 group-hover:scale-110 group-hover:rotate-3 transition-transform duration-500">
            <Target class="w-8 h-8 text-blue-600 dark:text-blue-400 transition-colors" stroke-width="1.5" />
          </div>
          <h3 class="text-2xl md:text-3xl font-semibold text-zinc-900 dark:text-zinc-100 mb-4 tracking-tight transition-colors">Nuestra Misión</h3>
          <p class="text-base md:text-lg text-zinc-600 dark:text-zinc-400 font-light leading-relaxed transition-colors">
            Transformar problemas de negocio complejos en soluciones de software elegantes, seguras y escalables. Creemos que la tecnología debe adaptarse a tu empresa, devolviéndote el control total sobre tus procesos operativos.
          </p>
        </div>

        <!-- Visión -->
        <div :ref="setCardRef" class="mission-card bg-gradient-to-br from-indigo-50/80 to-white/60 dark:from-indigo-950/30 dark:to-zinc-900/40 backdrop-blur-xl border border-indigo-100/80 dark:border-indigo-900/50 rounded-[2.5rem] p-8 lg:p-12 transition-all duration-700 hover:shadow-xl hover:shadow-indigo-500/10 hover:border-indigo-300/80 dark:hover:border-indigo-700/60 hover:-translate-y-2 shadow-2xl shadow-indigo-900/5 dark:shadow-none group relative overflow-hidden">
          <div class="absolute inset-0 bg-gradient-to-br from-indigo-500/5 to-purple-500/5 opacity-0 group-hover:opacity-100 transition-opacity duration-700"></div>
          <div class="w-16 h-16 bg-indigo-100/60 dark:bg-indigo-900/40 border border-indigo-200 dark:border-indigo-800/60 rounded-2xl flex items-center justify-center mb-8 shadow-lg shadow-indigo-500/5 group-hover:scale-110 group-hover:-rotate-3 transition-transform duration-500 relative z-10">
            <Lightbulb class="w-8 h-8 text-indigo-600 dark:text-indigo-400 transition-colors" stroke-width="1.5" />
          </div>
          <h3 class="text-2xl md:text-3xl font-semibold text-zinc-900 dark:text-zinc-100 mb-4 tracking-tight relative z-10 transition-colors">Nuestra Visión</h3>
          <p class="text-base md:text-lg text-zinc-600 dark:text-zinc-400 font-light leading-relaxed relative z-10 transition-colors">
            Ser el socio tecnológico de referencia para empresas que buscan liberarse de las limitaciones del software empaquetado. Queremos construir hoy la infraestructura digital que respaldará el crecimiento de mañana.
          </p>
        </div>
      </div>
    </section>

    <!-- 3. Orbital Community Section (Pantalla completa) -->
    <section ref="orbitalSectionRef" class="relative z-10 min-h-[100dvh] flex flex-col items-center justify-center px-4 md:px-8 w-full py-20">
      <div class="w-full max-w-7xl mx-auto flex flex-col items-center">
        <div class="text-center mb-16">
          <h3 class="text-4xl md:text-6xl font-serif text-zinc-900 dark:text-zinc-100 mb-6 tracking-tight transition-colors">Nuestra Comunidad</h3>
          <p class="text-xl md:text-2xl text-zinc-600 dark:text-zinc-400 max-w-3xl mx-auto font-light transition-colors">
            Conectando talento y tecnología para impulsar el futuro.
          </p>
        </div>
        <div class="w-full relative">
          <!-- Efecto de resplandor para la sección orbital -->
          <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[80%] h-[80%] bg-blue-500/10 dark:bg-blue-500/5 rounded-full blur-[100px] pointer-events-none z-0"></div>
          <OrbitalCommunity class="w-full relative z-10" />
        </div>
      </div>
    </section>
  </div>
</template>
