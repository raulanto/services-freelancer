<script setup lang="ts">
// NOTA: Asegúrate de tener instalado 'lucide-vue-next'
import { Search, PenTool, Code2, ShieldCheck, Wrench } from '@lucide/vue'
import { onMounted, onUnmounted, ref } from 'vue'
import gsap from 'gsap'
import ScrollTrigger from 'gsap/ScrollTrigger'

// ¡VITAL! Registrar el plugin ScrollTrigger para que las animaciones funcionen al hacer scroll
gsap.registerPlugin(ScrollTrigger)

// Referencias del DOM
const sectionRef = ref<HTMLElement | null>(null)
const stepsRef = ref<HTMLElement[]>([]) // En Vue 3.2+, esto se llena automáticamente si usas ref="stepsRef" en un v-for
const progressLineRef = ref<HTMLElement | null>(null)
let ctx: gsap.Context

// Definición de los pasos del proceso
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
  // Crear el contexto de GSAP para un manejo seguro de la limpieza de animaciones
  ctx = gsap.context(() => {
    
    // 1. Animación de la línea de progreso (Usando scaleY por rendimiento)
    gsap.fromTo(progressLineRef.value, 
      { 
        scaleY: 0, 
        transformOrigin: 'top center' // La línea crece hacia abajo
      },
      {
        scaleY: 1,
        ease: 'none', // Sincronizado perfecto con el scroll
        scrollTrigger: {
          trigger: sectionRef.value,
          start: 'top 50%',
          end: 'bottom 80%',
          scrub: 0.5 // Un ligero retraso lo hace sentir más fluido
        }
      }
    )

    // 2. Animación escalonada para cada paso Individualmente
    stepsRef.value.forEach((step, index) => {
      // Obtenemos los elementos hijos dentro de este paso
      const iconBox = step.querySelector('.step-icon-box')
      const contentBox = step.querySelector('.step-content')

      // Usar un Timeline para sincronizar la entrada del icono y el texto
      const tl = gsap.timeline({
        scrollTrigger: {
          trigger: step,
          start: 'top 75%', // Inicia un poco antes de que llegue a la mitad
          toggleActions: 'play none none reverse' // Reproduce al entrar, reverse al salir
        }
      })

      // Animación del Icono con un ligero rebote (clases gestionadas por CSS, solo escala/rotación)
      tl.fromTo(iconBox, 
        { 
          scale: 0.8,
          rotation: -10 // Un giro sutil inicial
        },
        {
          scale: 1,
          rotation: 0,
          duration: 0.5,
          ease: 'back.out(1.5)' // Un pequeño rebote final
        }
      )
      
      // Animamos las clases custom para cambiar colores via ScrollTrigger
      ScrollTrigger.create({
          trigger: step,
          start: 'top 75%',
          onEnter: () => iconBox?.classList.add('is-active'),
          onLeaveBack: () => iconBox?.classList.remove('is-active'),
      })
      
      // Animación del Texto con efecto de desenfoque al entrar
      tl.fromTo(contentBox,
        { 
          opacity: 0, 
          x: 40, // Viene un poco más lejos
          filter: 'blur(8px)' // Inicia borroso
        },
        {
          opacity: 1,
          x: 0,
          filter: 'blur(0px)', // Termina nítido
          duration: 0.6,
          ease: 'power2.out'
        },
        '<0.1' // Inicia 0.1s después de que arranca el icono
      )
    })
  }, sectionRef.value!) // Non-null assertion para TS
})

onUnmounted(() => {
  // Limpieza total de GSAP y ScrollTriggers al desmontar el componente
  ctx?.revert()
})
</script>

<template>
  <section ref="sectionRef" class="relative w-full py-24 md:py-32 bg-neutral-50 dark:bg-[#0a0a0a] overflow-hidden transition-colors duration-300">
    
    <!-- Elemento decorativo de fondo: Un glow radial sutil -->
    <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[600px] h-[600px] bg-zinc-200/50 dark:bg-zinc-900/40 rounded-full blur-[120px] pointer-events-none transition-colors duration-300"></div>

    <div class="max-w-6xl mx-auto px-6 lg:px-8 flex flex-col lg:flex-row gap-16 lg:gap-24 relative z-10">
      
      <!-- Lado Izquierdo: Título Sticky (Mejorado visualmente) -->
      <div class="w-full lg:w-1/3 lg:sticky lg:top-40 h-fit">
        <div class="inline-flex items-center gap-2 px-3 py-1 mb-6 rounded-full bg-zinc-200 dark:bg-zinc-900 border border-zinc-300 dark:border-zinc-800 text-xs font-medium text-zinc-600 dark:text-zinc-400 tracking-wide transition-colors">
          <span class="w-2 h-2 rounded-full bg-emerald-500 shadow-[0_0_8px_rgba(16,185,129,0.6)]"></span>
          Proceso Estructurado
        </div>
        <h2 class="text-4xl md:text-5xl font-serif text-zinc-900 dark:text-zinc-100 mb-6 tracking-tight transition-colors">
          Metodología <span class="block italic text-zinc-500 dark:text-zinc-500 mt-2">de Trabajo</span>
        </h2>
        <p class="text-lg text-zinc-600 dark:text-zinc-400 font-light leading-relaxed transition-colors">
          Para proyectos grandes, el caos no es una opción. Detrás de cada sistema que construyo existe un orden riguroso y una arquitectura limpia que garantiza el éxito a largo plazo.
        </p>
      </div>

      <!-- Lado Derecho: La Línea de Tiempo Interactiva -->
      <div class="w-full lg:w-2/3 relative py-4 md:py-10">
        
        <!-- Línea de fondo inactiva (Más sutil) -->
        <div class="absolute left-8 md:left-10 top-0 bottom-0 w-[2px] bg-zinc-200 dark:bg-zinc-900 z-0 transition-colors"></div>
        
        <!-- Línea de progreso (Animada con Gradiente y Brillo) -->
        <div ref="progressLineRef" class="absolute left-8 md:left-10 top-0 bottom-0 w-[2px] bg-gradient-to-b from-zinc-800 via-zinc-400 to-zinc-200 dark:from-zinc-200 dark:via-zinc-600 dark:to-zinc-900 z-0 shadow-[0_0_15px_rgba(0,0,0,0.1)] dark:shadow-[0_0_15px_rgba(228,228,231,0.5)]"></div>

        <div class="space-y-20 relative z-10">
          <!-- Al usar ref="stepsRef" dentro del v-for, Vue 3.2+ llena el array automáticamente -->
          <div 
            v-for="(step, index) in steps" 
            :key="index"
            ref="stepsRef"
            class="flex items-start gap-6 md:gap-10 group"
          >
            <!-- Indicador e Icono (Glassmorphism & Glow) -->
            <div class="relative flex flex-col items-center shrink-0">
              <div class="step-icon-box w-16 h-16 md:w-20 md:h-20 rounded-2xl backdrop-blur-md bg-zinc-100/80 dark:bg-zinc-900/40 border border-zinc-200 dark:border-zinc-800 flex items-center justify-center z-10 shadow-sm dark:shadow-xl transition-all duration-500 text-zinc-400 dark:text-zinc-600">
                <component :is="step.icon" class="w-7 h-7 md:w-8 md:h-8 transition-colors duration-500" stroke-width="1.5" />
              </div>
              <!-- Número flotante más limpio -->
              <span class="absolute -left-8 md:-left-12 top-4 md:top-6 text-zinc-300 dark:text-zinc-800 font-serif text-2xl md:text-3xl font-bold opacity-40 select-none transition-opacity group-hover:opacity-80">
                0{{ index + 1 }}
              </span>
            </div>

            <!-- Contenido del Paso (Tipografía más nítida) -->
            <div class="step-content pt-1 md:pt-3">
              <h3 class="text-xl md:text-2xl font-semibold text-zinc-800 dark:text-zinc-100 mb-3 tracking-tight transition-colors">{{ step.title }}</h3>
              <p class="text-base md:text-lg text-zinc-600 dark:text-zinc-400 leading-relaxed max-w-xl font-light transition-colors">
                {{ step.description }}
              </p>
            </div>
          </div>
        </div>

      </div>
    </div>
  </section>
</template>

<style scoped>
/* Transición de colores cuando es activado por ScrollTrigger */
.step-icon-box {
  transition: background-color 0.5s, border-color 0.5s, color 0.5s, box-shadow 0.5s;
}

.step-icon-box.is-active {
  background-color: #ffffff;
  border-color: #d4d4d8;
  color: #18181b;
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
}

/* Modo oscuro explícito usando html.dark o .dark en un contenedor padre */
:global(.dark) .step-icon-box.is-active {
  background-color: rgba(39, 39, 42, 0.8); /* zinc-800/80 */
  border-color: rgba(113, 113, 122, 0.3); /* zinc-500/30 */
  color: #f4f4f5; /* zinc-100 */
  box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.5);
}
</style>