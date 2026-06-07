<script setup lang="ts">
import { Search, PenTool, Code2, ShieldCheck, Wrench } from '@lucide/vue'
import { onMounted, onUnmounted, ref } from 'vue'
import gsap from 'gsap'

const sectionRef = ref(null)
const stepsRef = ref([])
const progressLineRef = ref(null)
let ctx: gsap.Context

const steps = [
  {
    title: 'Descubrimiento',
    description: 'Análisis profundo del negocio, entender el problema real y realizar un levantamiento de requerimientos preciso antes de escribir una sola línea de código.',
    icon: Search
  },
  {
    title: 'Arquitectura y Diseño',
    description: 'Definición de la estructura del sistema, diagramado de bases de datos escalables y maquetado de interfaces (UI/UX) centradas en el usuario.',
    icon: PenTool
  },
  {
    title: 'Desarrollo y Entregas Ágiles',
    description: 'Trabajo bajo flujos estructurados con entregas por módulos funcionales y control de versiones seguro, dándote visibilidad constante del avance.',
    icon: Code2
  },
  {
    title: 'Pruebas y Despliegue',
    description: 'Riguroso aseguramiento de calidad (QA). Detectamos y corregimos errores antes de salir a producción para un lanzamiento impecable.',
    icon: ShieldCheck
  },
  {
    title: 'Mantenimiento y Evolución',
    description: 'Acompañamiento continuo post-lanzamiento. Monitoreo de estabilidad, actualizaciones críticas y escalado de la arquitectura según tu crecimiento.',
    icon: Wrench
  }
]

onMounted(() => {
  ctx = gsap.context(() => {
    // Animate the vertical line drawing down
    gsap.fromTo(progressLineRef.value, 
      { height: '0%' },
      {
        height: '100%',
        ease: 'none',
        scrollTrigger: {
          trigger: sectionRef.value,
          start: 'top 50%',
          end: 'bottom 80%',
          scrub: true
        }
      }
    )

    // Animate each step highlighting as you scroll past them
    stepsRef.value.forEach((step, index) => {
      // The icon container
      const iconBox = step.querySelector('.step-icon-box')
      // The text content
      const contentBox = step.querySelector('.step-content')

      gsap.fromTo(iconBox, 
        { backgroundColor: 'rgba(23, 23, 23, 1)', borderColor: 'rgba(38, 38, 38, 1)', color: 'rgba(115, 115, 115, 1)', scale: 0.8 },
        {
          backgroundColor: 'rgba(38, 38, 38, 1)',
          borderColor: 'rgba(115, 115, 115, 1)',
          color: 'rgba(245, 245, 245, 1)',
          scale: 1,
          duration: 0.5,
          scrollTrigger: {
            trigger: step,
            start: 'top 60%',
            toggleActions: 'play none none reverse'
          }
        }
      )

      gsap.fromTo(contentBox,
        { opacity: 0.3, x: 20 },
        {
          opacity: 1,
          x: 0,
          duration: 0.5,
          scrollTrigger: {
            trigger: step,
            start: 'top 60%',
            toggleActions: 'play none none reverse'
          }
        }
      )
    })
  }, sectionRef.value)
})

onUnmounted(() => {
  ctx?.revert()
})
</script>

<template>
  <section ref="sectionRef" class="relative w-full py-32 bg-neutral-950 overflow-hidden">
    <div class="max-w-6xl mx-auto px-4 flex flex-col lg:flex-row gap-16 lg:gap-24 relative z-10">
      
      <!-- Sticky Left Side: Title -->
      <div class="w-full lg:w-1/3 lg:sticky lg:top-40 h-fit">
        <h2 class="text-4xl md:text-6xl font-serif text-neutral-100 mb-6 tracking-tight">
          Metodología <span class="block italic text-neutral-400 mt-2">de Trabajo</span>
        </h2>
        <p class="text-lg text-neutral-400 font-light leading-relaxed">
          Para proyectos grandes, el caos no es una opción. Detrás de cada sistema que construyo existe un orden riguroso y una arquitectura limpia que garantiza el éxito a largo plazo.
        </p>
      </div>

      <!-- Right Side: The Timeline Process -->
      <div class="w-full lg:w-2/3 relative py-10">
        
        <!-- The Background Line -->
        <div class="absolute left-8 md:left-10 top-0 bottom-0 w-[2px] bg-neutral-900 z-0"></div>
        
        <!-- The Progress Line (Animated) -->
        <div ref="progressLineRef" class="absolute left-8 md:left-10 top-0 w-[2px] bg-neutral-400 z-0 shadow-[0_0_15px_rgba(163,163,163,0.5)]"></div>

        <div class="space-y-24 relative z-10">
          <div 
            v-for="(step, index) in steps" 
            :key="index"
            :ref="el => stepsRef.push(el)"
            class="flex items-start gap-8 md:gap-12 group"
          >
            <!-- Number & Icon Indicator -->
            <div class="relative flex flex-col items-center">
              <div class="step-icon-box w-16 h-16 md:w-20 md:h-20 rounded-2xl bg-neutral-900 border border-neutral-800 flex items-center justify-center shrink-0 z-10 shadow-xl transition-colors duration-300">
                <component :is="step.icon" class="w-7 h-7 md:w-8 md:h-8" />
              </div>
              <span class="absolute -left-6 top-4 md:top-6 text-neutral-700 font-serif text-3xl font-bold opacity-30 select-none">
                0{{ index + 1 }}
              </span>
            </div>

            <!-- Content -->
            <div class="step-content pt-2 md:pt-4">
              <h3 class="text-2xl md:text-3xl font-medium text-white mb-4">{{ step.title }}</h3>
              <p class="text-lg text-neutral-400 leading-relaxed max-w-xl">
                {{ step.description }}
              </p>
            </div>
          </div>
        </div>

      </div>
    </div>
  </section>
</template>
