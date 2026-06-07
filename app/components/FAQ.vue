<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import { Plus, Minus } from '@lucide/vue'
import gsap from 'gsap'

const sectionRef = ref(null)
const titleRef = ref(null)
const openIndex = ref<number | null>(null)
let ctx: gsap.Context

const faqs = [
  {
    q: '¿Cuánto tiempo tarda en desarrollarse una aplicación a medida?',
    a: 'Depende del alcance. Un sistema core de gestión (ERP/CRM básico) tarda entre 8 y 16 semanas. Aplicaciones más complejas con integraciones avanzadas pueden extenderse entre 4 y 6 meses. Trabajamos con entregas por módulos, lo que significa que ves resultados funcionales desde las primeras semanas, no al final del proyecto.',
    label: 'Tiempos'
  },
  {
    q: '¿De quién es el código fuente una vez terminado el proyecto?',
    a: 'Tuyo, al 100%. Una vez completado el desarrollo y gestionado el pago final, te entrego el repositorio completo con toda la documentación técnica. No hay licencias ocultas ni dependencias de mi parte. El software es un activo que pertenece íntegramente a tu empresa.',
    label: 'Propiedad'
  },
  {
    q: '¿Cómo se maneja el soporte y mantenimiento mensual?',
    a: 'Manejamos planes de mantenimiento post-lanzamiento que cubren: monitoreo de estabilidad del servidor, corrección de bugs críticos con tiempos de respuesta garantizados, actualizaciones de seguridad de dependencias y soporte técnico directo por canal dedicado. Los planes se adaptan a la criticidad del sistema de tu negocio.',
    label: 'Soporte'
  },
  {
    q: '¿Qué pasa si mi negocio crece y necesito más funciones?',
    a: 'Para eso fue diseñada la arquitectura desde el día uno. El sistema se construye pensando en la escalabilidad desde la base de datos hasta el frontend. Añadir nuevos módulos, integraciones con terceros o aumentar la capacidad del servidor es un proceso limpio y controlado, sin necesidad de reescribir desde cero.',
    label: 'Escalabilidad'
  },
]

const toggle = (i: number) => {
  const prev = openIndex.value
  openIndex.value = prev === i ? null : i

  // Animate the answer panel
  const panels = sectionRef.value?.querySelectorAll('.faq-panel')
  if (!panels) return

  panels.forEach((panel, idx) => {
    if (idx === i && prev !== i) {
      gsap.fromTo(panel, { height: 0, opacity: 0 }, { height: 'auto', opacity: 1, duration: 0.4, ease: 'power3.out' })
    } else if (idx === prev && prev !== null) {
      gsap.to(panel, { height: 0, opacity: 0, duration: 0.3, ease: 'power3.in' })
    }
  })
}

onMounted(() => {
  ctx = gsap.context(() => {
    gsap.from(titleRef.value, {
      y: 60, opacity: 0, duration: 1, ease: 'power3.out',
      scrollTrigger: { trigger: sectionRef.value, start: 'top 75%' }
    })

    const items = sectionRef.value?.querySelectorAll('.faq-item')
    gsap.from(items, {
      y: 40,
      opacity: 0,
      duration: 0.6,
      stagger: 0.1,
      ease: 'power3.out',
      scrollTrigger: { trigger: sectionRef.value, start: 'top 65%' }
    })
  }, sectionRef.value)
})

onUnmounted(() => ctx?.revert())
</script>

<template>
  <section ref="sectionRef" class="relative w-full py-32 bg-neutral-950 overflow-hidden">
    <!-- Decorative grid lines -->
    <div class="absolute inset-0 pointer-events-none opacity-[0.03]"
      style="background-image: linear-gradient(rgba(255,255,255,0.8) 1px, transparent 1px), linear-gradient(90deg, rgba(255,255,255,0.8) 1px, transparent 1px); background-size: 80px 80px;">
    </div>
    <div class="absolute inset-0 bg-[radial-gradient(ellipse_60%_60%_at_50%_100%,rgba(255,255,255,0.03)_0%,transparent_70%)] pointer-events-none"></div>

    <div class="max-w-5xl mx-auto px-4 relative z-10">

      <!-- Header -->
      <div ref="titleRef" class="flex flex-col md:flex-row md:items-end justify-between gap-8 mb-20">
        <div>
          <p class="text-neutral-500 font-mono text-xs tracking-[0.25em] uppercase mb-4">FAQ</p>
          <h2 class="text-4xl md:text-6xl font-serif text-neutral-100 tracking-tight leading-tight">
            Preguntas<br />
            <span class="italic text-neutral-400">frecuentes</span>
          </h2>
        </div>
        <p class="text-neutral-400 font-light max-w-xs leading-relaxed text-base md:text-right">
          Resolvemos las dudas más comunes antes de la primera llamada para que puedas tomar decisiones con información real.
        </p>
      </div>

      <!-- FAQ Items -->
      <div class="divide-y divide-neutral-800/60">
        <div
          v-for="(faq, i) in faqs"
          :key="i"
          class="faq-item group"
        >
          <!-- Question row -->
          <button
            class="w-full flex items-start gap-6 py-8 text-left transition-colors duration-300 group cursor-pointer"
            @click="toggle(i)"
          >
            <!-- Index label pill -->
            <span class="shrink-0 mt-0.5 text-xs font-mono text-neutral-600 border border-neutral-800 rounded-lg px-2 py-1 transition-colors duration-300 group-hover:border-neutral-600 group-hover:text-neutral-400">
              {{ String(i + 1).padStart(2, '0') }}
            </span>

            <!-- Question -->
            <span class="flex-1 text-lg md:text-xl font-medium transition-colors duration-300"
              :class="openIndex === i ? 'text-white' : 'text-neutral-300 group-hover:text-white'">
              {{ faq.q }}
            </span>

            <!-- Category tag -->
            <span class="hidden md:block shrink-0 text-xs font-medium text-neutral-600 bg-neutral-900 border border-neutral-800 rounded-full px-3 py-1 transition-all duration-300"
              :class="openIndex === i ? 'text-neutral-300 border-neutral-600' : ''">
              {{ faq.label }}
            </span>

            <!-- Icon -->
            <div class="shrink-0 w-8 h-8 rounded-full flex items-center justify-center border transition-all duration-300 mt-0.5"
              :class="openIndex === i
                ? 'bg-neutral-100 border-neutral-100 text-neutral-900'
                : 'bg-transparent border-neutral-700 text-neutral-400 group-hover:border-neutral-500 group-hover:text-neutral-200'">
              <Plus v-if="openIndex !== i" class="size-4" />
              <Minus v-else class="size-4" />
            </div>
          </button>

          <!-- Answer panel (GSAP-controlled height) -->
          <div class="faq-panel overflow-hidden h-0 opacity-0">
            <div class="pb-8 pl-[3.25rem] md:pl-[4rem]">
              <p class="text-neutral-400 leading-relaxed text-base md:text-lg font-light max-w-3xl">
                {{ faq.a }}
              </p>
            </div>
          </div>
        </div>
      </div>

      <!-- CTA below FAQ -->
      <div class="mt-20 text-center">
        <p class="text-neutral-500 mb-6 text-base">¿Tienes una pregunta específica que no está aquí?</p>
        <button class="group inline-flex items-center gap-3 bg-neutral-900 border border-neutral-700 hover:border-neutral-500 hover:bg-neutral-800 text-neutral-200 rounded-full px-8 py-4 text-base font-medium transition-all duration-300">
          <span>Agendar una llamada directa</span>
          <span class="w-6 h-6 rounded-full bg-neutral-700 group-hover:bg-neutral-600 flex items-center justify-center transition-colors duration-300">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" class="w-3 h-3">
              <path fill-rule="evenodd" d="M5.22 14.78a.75.75 0 0 0 1.06 0l7.22-7.22v5.69a.75.75 0 0 0 1.5 0v-7.5a.75.75 0 0 0-.75-.75h-7.5a.75.75 0 0 0 0 1.5h5.69l-7.22 7.22a.75.75 0 0 0 0 1.06Z" clip-rule="evenodd" />
            </svg>
          </span>
        </button>
      </div>

    </div>
  </section>
</template>
