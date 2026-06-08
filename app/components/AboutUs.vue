<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import gsap from 'gsap'
import ScrollTrigger from 'gsap/ScrollTrigger'
import { Target, Lightbulb } from '@lucide/vue'
import OrbitalCommunity from '~/components/OrbitalCommunity.vue'

gsap.registerPlugin(ScrollTrigger)

const sectionRef = ref<HTMLElement | null>(null)
const cardsRef = ref<HTMLElement[]>([])
let ctx: gsap.Context

onMounted(() => {
  ctx = gsap.context(() => {
    // Animación de los bloques de Misión/Visión
    gsap.from(cardsRef.value, {
      y: 60,
      opacity: 0,
      duration: 0.8,
      stagger: 0.2,
      ease: 'power3.out',
      scrollTrigger: {
        trigger: sectionRef.value,
        start: 'top 75%',
        toggleActions: 'play none none reverse'
      }
    })
  }, sectionRef.value!)
})

onUnmounted(() => {
  ctx?.revert()
})
</script>

<template>
  <section ref="sectionRef" class="relative w-full py-24 md:py-32 bg-neutral-50 dark:bg-[#0a0a0a] overflow-hidden transition-colors duration-300">
    <!-- Glow radial de fondo -->
    <div class="absolute top-0 left-1/2 -translate-x-1/2 w-[800px] h-[800px] bg-zinc-200/50 dark:bg-zinc-800/10 rounded-full blur-[120px] pointer-events-none transition-colors"></div>

    <div class="max-w-6xl mx-auto px-6 lg:px-8 relative z-10">
      
      <!-- Encabezado -->
      <div class="text-center mb-16 md:mb-24">
        <div class="inline-flex items-center gap-2 px-3 py-1 mb-6 rounded-full bg-zinc-200 dark:bg-zinc-900 border border-zinc-300 dark:border-zinc-800 text-xs font-medium text-zinc-600 dark:text-zinc-400 tracking-wide transition-colors">
          <span class="w-2 h-2 rounded-full bg-blue-500 shadow-[0_0_8px_rgba(59,130,246,0.6)]"></span>
          Sobre Nosotros
        </div>
        <h2 class="text-4xl md:text-5xl font-serif text-zinc-900 dark:text-zinc-100 mb-6 tracking-tight transition-colors">
          Quiénes <span class="italic text-zinc-500">Somos</span>
        </h2>
        <p class="text-lg text-zinc-600 dark:text-zinc-400 max-w-2xl mx-auto font-light leading-relaxed transition-colors">
          No somos una agencia tradicional que entrega código y desaparece. Somos aliados tecnológicos comprometidos con el éxito y la escalabilidad de tu negocio.
        </p>
      </div>

      <!-- Misión y Visión -->
      <div class="grid md:grid-cols-2 gap-6 lg:gap-10 mb-32">
        <!-- Misión -->
        <div ref="cardsRef" class="bg-white/60 dark:bg-zinc-900/40 backdrop-blur-md border border-zinc-200 dark:border-zinc-800/80 rounded-3xl p-8 md:p-10 transition-all duration-500 hover:bg-white/80 dark:hover:bg-zinc-900/60 hover:border-zinc-300 dark:hover:border-zinc-700/80 shadow-xl dark:shadow-none group">
          <div class="w-14 h-14 bg-zinc-100 dark:bg-zinc-950 border border-zinc-200 dark:border-zinc-800 rounded-2xl flex items-center justify-center mb-6 shadow-md dark:shadow-lg group-hover:scale-110 transition-transform duration-300">
            <Target class="w-6 h-6 text-zinc-600 dark:text-zinc-300 transition-colors" stroke-width="1.5" />
          </div>
          <h3 class="text-2xl font-semibold text-zinc-900 dark:text-zinc-100 mb-4 tracking-tight transition-colors">Nuestra Misión</h3>
          <p class="text-zinc-600 dark:text-zinc-400 font-light leading-relaxed transition-colors">
            Transformar problemas de negocio complejos en soluciones de software elegantes, seguras y escalables. Creemos que la tecnología debe adaptarse a tu empresa, devolviéndote el control total sobre tus procesos operativos.
          </p>
        </div>

        <!-- Visión -->
        <div ref="cardsRef" class="bg-white/60 dark:bg-zinc-900/40 backdrop-blur-md border border-zinc-200 dark:border-zinc-800/80 rounded-3xl p-8 md:p-10 transition-all duration-500 hover:bg-white/80 dark:hover:bg-zinc-900/60 hover:border-zinc-300 dark:hover:border-zinc-700/80 shadow-xl dark:shadow-none group relative overflow-hidden">
          <div class="absolute inset-0 bg-gradient-to-br from-blue-500/5 to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-500"></div>
          <div class="w-14 h-14 bg-zinc-100 dark:bg-zinc-950 border border-zinc-200 dark:border-zinc-800 rounded-2xl flex items-center justify-center mb-6 shadow-md dark:shadow-lg group-hover:scale-110 transition-transform duration-300 relative z-10">
            <Lightbulb class="w-6 h-6 text-zinc-600 dark:text-zinc-300 transition-colors" stroke-width="1.5" />
          </div>
          <h3 class="text-2xl font-semibold text-zinc-900 dark:text-zinc-100 mb-4 tracking-tight relative z-10 transition-colors">Nuestra Visión</h3>
          <p class="text-zinc-600 dark:text-zinc-400 font-light leading-relaxed relative z-10 transition-colors">
            Ser el socio tecnológico de referencia para empresas que buscan liberarse de las limitaciones del software empaquetado. Queremos construir hoy la infraestructura digital que respaldará el crecimiento de mañana.
          </p>
        </div>
      </div>

      <!-- Orbital Community Section -->
      <OrbitalCommunity />

    </div>
  </section>
</template>
