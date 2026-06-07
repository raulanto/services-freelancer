<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import gsap from 'gsap'
import ScrollTrigger from 'gsap/ScrollTrigger'
import { Target, Lightbulb, Palette, Mail, ArrowRight } from '@lucide/vue'
import Skibg from '~/components/Skibg.vue'

gsap.registerPlugin(ScrollTrigger)

const sectionRef = ref<HTMLElement | null>(null)
const cardsRef = ref<HTMLElement[]>([])
const orbitalRef = ref<HTMLElement | null>(null)
const avatarsRef = ref<HTMLElement[]>([])
let ctx: gsap.Context

// Generamos posiciones matemáticas exactas (r*cos(θ), r*sin(θ)) para que se asienten perfectamente en las líneas
const orbitalAvatars = [
  // Círculo exterior (r=50%)
  { top: '50%', left: '100%', size: 'w-10 h-10', img: 'https://api.dicebear.com/7.x/avataaars/svg?seed=A1' }, // 0 deg
  { top: '97.5%', left: '65.4%', size: 'w-8 h-8', img: 'https://api.dicebear.com/7.x/avataaars/svg?seed=A2' }, // 72 deg
  { top: '79.3%', left: '9.5%', size: 'w-12 h-12', img: 'https://api.dicebear.com/7.x/avataaars/svg?seed=A3' }, // 144 deg
  { top: '20.6%', left: '9.5%', size: 'w-10 h-10', img: 'https://api.dicebear.com/7.x/avataaars/svg?seed=A4' }, // 216 deg
  { top: '2.4%', left: '65.4%', size: 'w-8 h-8', img: 'https://api.dicebear.com/7.x/avataaars/svg?seed=A5' }, // 288 deg

  // Círculo medio (r=35%)
  { top: '74.7%', left: '74.7%', size: 'w-10 h-10', img: 'https://api.dicebear.com/7.x/avataaars/svg?seed=M1' }, // 45 deg
  { top: '74.7%', left: '25.2%', size: 'w-8 h-8', img: 'https://api.dicebear.com/7.x/avataaars/svg?seed=M2' }, // 135 deg
  { top: '25.2%', left: '25.2%', size: 'w-12 h-12', img: 'https://api.dicebear.com/7.x/avataaars/svg?seed=M3' }, // 225 deg
  { top: '25.2%', left: '74.7%', size: 'w-8 h-8', img: 'https://api.dicebear.com/7.x/avataaars/svg?seed=M4' }, // 315 deg

  // Círculo interior (r=20%)
  { top: '70%', left: '50%', size: 'w-12 h-12', img: 'https://api.dicebear.com/7.x/avataaars/svg?seed=I1' }, // 90 deg
  { top: '40%', left: '32.6%', size: 'w-10 h-10', img: 'https://api.dicebear.com/7.x/avataaars/svg?seed=I2' }, // 210 deg
  { top: '40%', left: '67.3%', size: 'w-8 h-8', img: 'https://api.dicebear.com/7.x/avataaars/svg?seed=I3' }, // 330 deg
]

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

    // Animación del contenedor orbital
    gsap.from(orbitalRef.value, {
      scale: 0.8,
      opacity: 0,
      duration: 1,
      ease: 'power3.out',
      scrollTrigger: {
        trigger: '.orbital-section',
        start: 'top 80%',
      }
    })

    // Animación escalonada (pop-in) de los avatares
    gsap.from(avatarsRef.value, {
      scale: 0,
      opacity: 0,
      duration: 0.6,
      stagger: 0.05,
      ease: 'back.out(2)',
      scrollTrigger: {
        trigger: '.orbital-section',
        start: 'top 70%',
      }
    })

    // Rotación infinita suave para todo el sistema orbital
    gsap.to(orbitalRef.value, {
      rotation: 360,
      duration: 120, // muy lento
      repeat: -1,
      ease: 'none'
    })

    // Contra-rotación de los avatares para que no queden boca abajo
    gsap.to(avatarsRef.value, {
      rotation: -360,
      duration: 120,
      repeat: -1,
      ease: 'none'
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
      <div class="orbital-section grid lg:grid-cols-2 gap-16 lg:gap-8 items-center mt-20">
        <!-- Izquierda: Textos y CTA -->
        <div class="order-2 lg:order-1 text-center lg:text-left z-10">
          <h3 class="text-4xl md:text-5xl font-serif text-zinc-900 dark:text-white mb-6 tracking-tight transition-colors">
            La diferencia <span class="italic text-zinc-500 block mt-2">de ser</span>
          </h3>
          <p class="text-lg text-zinc-600 dark:text-zinc-400 mb-10 leading-relaxed max-w-lg mx-auto lg:mx-0 font-light transition-colors">
            Una Comunidad de colaboradores y clientes. Trabajamos junto a diseñadores, desarrolladores expertos y empresas excepcionales para construir el mejor ecosistema tecnológico.
          </p>
          
          <!-- Estadísticas -->
          <div class="flex items-center justify-center lg:justify-start gap-12 mb-10">
            <div>
              <div class="text-3xl md:text-4xl font-semibold text-zinc-900 dark:text-white mb-1 transition-colors">15+</div>
              <div class="text-zinc-500 text-sm font-medium uppercase tracking-wider transition-colors">Proyectos</div>
            </div>
            <div>
              <div class="text-3xl md:text-4xl font-semibold text-zinc-900 dark:text-white mb-1 transition-colors">100%</div>
              <div class="text-zinc-500 text-sm font-medium uppercase tracking-wider transition-colors">A Medida</div>
            </div>
            <div>
              <div class="text-3xl md:text-4xl font-semibold text-zinc-900 dark:text-white mb-1 transition-colors">24/7</div>
              <div class="text-zinc-500 text-sm font-medium uppercase tracking-wider transition-colors">Soporte</div>
            </div>
          </div>

          <button class="inline-flex items-center gap-2 px-6 py-3 rounded-full bg-zinc-900 dark:bg-zinc-100 text-white dark:text-zinc-900 font-medium hover:bg-zinc-800 dark:hover:bg-white transition-colors duration-300">
            Conocer al equipo
            <ArrowRight class="w-4 h-4" />
          </button>
        </div>

        <!-- Derecha: Orbital con Skibg -->
        <div class="order-1 lg:order-2 relative w-full aspect-square max-w-[500px] mx-auto flex items-center justify-center">
          
          <!-- Fondo Skibg con máscara radial para difuminar los bordes -->
          <div class="absolute inset-0 z-0 opacity-20 dark:opacity-40 mix-blend-multiply dark:mix-blend-screen mask-radial transition-opacity">
            <ClientOnly>
              <Skibg class="scale-[1.5]" />
            </ClientOnly>
          </div>

          <!-- Sistema Orbital -->
          <div ref="orbitalRef" class="relative w-[90%] h-[90%] z-10 flex items-center justify-center">
            
            <!-- Círculos concéntricos mejorados (Bordes punteados y sólidos mezclados) -->
            <div class="absolute w-full h-full rounded-full border border-zinc-300 dark:border-zinc-800 border-dashed opacity-50 transition-colors"></div>
            <div class="absolute w-[70%] h-[70%] rounded-full border border-zinc-200 dark:border-zinc-700/60 shadow-inner dark:shadow-[0_0_30px_inset_rgba(255,255,255,0.02)] transition-colors"></div>
            <div class="absolute w-[40%] h-[40%] rounded-full border border-zinc-300 dark:border-zinc-600/40 border-dashed shadow-[0_0_20px_rgba(0,0,0,0.03)] dark:shadow-[0_0_20px_rgba(255,255,255,0.03)] transition-colors"></div>

            <!-- Avatar Central (El Líder / Yo) -->
            <div class="absolute w-20 h-20 bg-white dark:bg-zinc-950 border border-zinc-200 dark:border-zinc-600 rounded-full z-20 flex items-center justify-center shadow-2xl dark:shadow-[0_0_40px_rgba(59,130,246,0.15)] overflow-hidden transition-colors">
              <img src="https://api.dicebear.com/7.x/avataaars/svg?seed=Raul&backgroundColor=27272a" class="w-full h-full object-cover scale-110" />
            </div>

            <!-- Avatares Orbitando -->
            <div 
              v-for="(avatar, i) in orbitalAvatars" 
              :key="i"
              :ref="el => avatarsRef.push(el)"
              class="absolute -translate-x-1/2 -translate-y-1/2 bg-white dark:bg-zinc-950 rounded-full border border-zinc-200 dark:border-zinc-700/80 shadow-xl dark:shadow-2xl flex items-center justify-center transition-all duration-300 hover:scale-125 hover:border-zinc-400 dark:hover:border-zinc-400 hover:z-30 group"
              :class="avatar.size"
              :style="{ top: avatar.top, left: avatar.left }"
            >
              <div class="absolute inset-0 rounded-full bg-blue-500/10 opacity-0 group-hover:opacity-100 transition-opacity"></div>
              <img :src="avatar.img" class="w-full h-full object-cover rounded-full opacity-70 group-hover:opacity-100 transition-opacity" />
            </div>
            
          </div>
        </div>
      </div>

    </div>
  </section>
</template>

<style scoped>
/* Máscara para hacer que el componente Skibg se desvanezca en los bordes */
.mask-radial {
  mask-image: radial-gradient(circle at center, black 30%, transparent 70%);
  -webkit-mask-image: radial-gradient(circle at center, black 30%, transparent 70%);
}
</style>
