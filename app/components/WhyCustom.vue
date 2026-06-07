<script setup lang="ts">
import { XCircle, CheckCircle, Boxes, ShieldCheck, Users, LineChart } from '@lucide/vue'
import Skibg from '~/components/Skibg.vue'
import { onMounted, onUnmounted, ref } from 'vue'
import gsap from 'gsap'

const sectionRef = ref(null)
const bgRef = ref(null)
const card1Ref = ref(null)
const card2Ref = ref(null)
let ctx: gsap.Context

onMounted(() => {
  // Pre-hide cards immediately to prevent flash
  gsap.set([card1Ref.value, card2Ref.value], { y: 60, opacity: 0 })

  ctx = gsap.context(() => {
    // Parallax background — scrub is fine here, it's purely positional
    gsap.to(bgRef.value, {
      yPercent: 20,
      ease: 'none',
      scrollTrigger: {
        trigger: sectionRef.value,
        start: 'top bottom',
        end: 'bottom top',
        scrub: true
      }
    })

    // Card 1: clean play/reverse, no scrub
    gsap.to(card1Ref.value, {
      y: 0,
      opacity: 1,
      duration: 0.8,
      ease: 'power3.out',
      scrollTrigger: {
        trigger: sectionRef.value,
        start: 'top 75%',
        toggleActions: 'play none none reverse'
      }
    })

    // Card 2: slight delay for stagger feel
    gsap.to(card2Ref.value, {
      y: 0,
      opacity: 1,
      duration: 0.8,
      delay: 0.15,
      ease: 'power3.out',
      scrollTrigger: {
        trigger: sectionRef.value,
        start: 'top 75%',
        toggleActions: 'play none none reverse'
      }
    })
  })
})

onUnmounted(() => {
  ctx?.revert()
})
</script>

<template>
  <section ref="sectionRef" class="relative w-full py-16 overflow-hidden h-screen flex flex-col justify-center">
    <!-- Skibg Background -->
    <div ref="bgRef" class="absolute inset-0 w-full h-full z-0 pointer-events-none scale-[1.2] -top-[10%]">
      <ClientOnly>
        <Skibg 
        />
      </ClientOnly>
    </div>

    <div class="w-full max-w-5xl mx-auto px-4 relative z-10">
      <div class="text-center mb-10">
      <h2 class="text-3xl md:text-5xl font-serif text-neutral-900 dark:text-neutral-100 mb-4 tracking-tight transition-colors duration-300">
        El problema del <span class="italic text-neutral-500 dark:text-neutral-400">software genérico</span>
      </h2>
      <p class="text-base md:text-lg text-neutral-600 dark:text-neutral-400 max-w-2xl mx-auto font-light transition-colors duration-300">
        La mayoría de empresas dudan entre pagar suscripciones mensuales o construir un sistema propio. Descubre por qué un ecosistema a medida es la clave para escalar.
      </p>
    </div>

    <div class="grid md:grid-cols-2 gap-6 items-stretch">
      <!-- El Problema: Software Genérico -->
      <div ref="card1Ref" class="bg-white/60 dark:bg-neutral-900/40 border border-neutral-200 dark:border-neutral-800/80 rounded-[2rem] p-8 backdrop-blur-md relative overflow-hidden group transition-all duration-500 hover:bg-white/80 dark:hover:bg-neutral-900/60 hover:-translate-y-2 hover:shadow-[0_20px_40px_-15px_rgba(248,113,113,0.08)] shadow-xl dark:shadow-none">
        <div class="absolute top-0 right-0 -mt-10 -mr-10 w-40 h-40 bg-red-500/5 rounded-full blur-3xl group-hover:bg-red-500/10 transition-colors duration-500 pointer-events-none"></div>
        <div class="flex items-center gap-4 mb-8 relative z-10">
          <div class="p-3 bg-neutral-100 dark:bg-neutral-950 rounded-2xl border border-neutral-200 dark:border-neutral-800">
            <XCircle class="size-6 text-red-500 dark:text-red-400/80" />
          </div>
          <h3 class="text-2xl font-medium text-neutral-900 dark:text-neutral-300 transition-colors">SaaS Genérico</h3>
        </div>
        
        <ul class="space-y-6 relative z-10">
          <li class="flex items-start gap-4">
            <div class="mt-1 p-2 bg-neutral-100 dark:bg-neutral-950 rounded-lg"><Boxes class="size-4 text-neutral-500 dark:text-neutral-500" /></div>
            <div>
              <h4 class="text-neutral-800 dark:text-neutral-300 font-medium mb-2 transition-colors">Información fragmentada</h4>
              <p class="text-neutral-600 dark:text-neutral-500 text-sm leading-relaxed transition-colors">Terminas usando 5 herramientas distintas que no se comunican entre sí. Datos duplicados, procesos manuales y pérdida de tiempo.</p>
            </div>
          </li>
          <li class="flex items-start gap-4">
            <div class="mt-1 p-2 bg-neutral-100 dark:bg-neutral-950 rounded-lg"><Users class="size-4 text-neutral-500 dark:text-neutral-500" /></div>
            <div>
              <h4 class="text-neutral-800 dark:text-neutral-300 font-medium mb-2 transition-colors">Licencias por usuario</h4>
              <p class="text-neutral-600 dark:text-neutral-500 text-sm leading-relaxed transition-colors">Mientras más crece tu equipo, más cara se vuelve la suscripción mensual. Escalar se convierte en un coste fijo insostenible.</p>
            </div>
          </li>
          <li class="flex items-start gap-4">
            <div class="mt-1 p-2 bg-neutral-100 dark:bg-neutral-950 rounded-lg"><LineChart class="size-4 text-neutral-500 dark:text-neutral-500" /></div>
            <div>
              <h4 class="text-neutral-800 dark:text-neutral-300 font-medium mb-2 transition-colors">Limita tu crecimiento</h4>
              <p class="text-neutral-600 dark:text-neutral-500 text-sm leading-relaxed transition-colors">Debes adaptar la lógica de tu negocio a lo que el software permite, no al revés. Tus procesos únicos se pierden.</p>
            </div>
          </li>
        </ul>
      </div>

      <!-- La Solución: Software a Medida -->
      <div ref="card2Ref" class="bg-gradient-to-b from-white/90 to-neutral-50/90 dark:from-neutral-900/90 dark:to-neutral-950/90 border border-neutral-200 dark:border-neutral-700/60 rounded-[2rem] p-8 backdrop-blur-xl relative overflow-hidden shadow-[0_0_40px_-15px_rgba(0,0,0,0.1)] dark:shadow-[0_0_40px_-15px_rgba(255,255,255,0.05)] group transition-all duration-500 hover:border-neutral-300 dark:hover:border-neutral-500/60 hover:-translate-y-2 hover:shadow-[0_20px_50px_-15px_rgba(0,0,0,0.15)] dark:hover:shadow-[0_20px_50px_-15px_rgba(255,255,255,0.1)]">
        <div class="absolute -top-24 -right-24 w-64 h-64 bg-neutral-900/5 dark:bg-white/5 rounded-full blur-3xl group-hover:bg-neutral-900/10 dark:group-hover:bg-white/10 transition-colors duration-500 pointer-events-none"></div>
        <div class="absolute inset-0 bg-gradient-to-br from-neutral-900/[0.03] dark:from-white/[0.03] to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-500 pointer-events-none"></div>
        <div class="flex items-center gap-4 mb-8 relative z-10">
          <div class="p-3 bg-white dark:bg-neutral-950 rounded-2xl border border-neutral-200 dark:border-neutral-700">
            <CheckCircle class="size-6 text-neutral-800 dark:text-neutral-200" />
          </div>
          <h3 class="text-2xl font-medium text-neutral-900 dark:text-white transition-colors">Ecosistema Propio</h3>
        </div>
        
        <ul class="space-y-6 relative z-10">
          <li class="flex items-start gap-4 group/item">
            <div class="mt-1 p-2 bg-white dark:bg-neutral-950 rounded-lg border border-neutral-200 dark:border-neutral-800"><ShieldCheck class="size-4 text-neutral-600 dark:text-neutral-300" /></div>
            <div>
              <h4 class="text-neutral-800 dark:text-neutral-100 font-medium mb-2 transition-colors">100% Adaptado a tu lógica</h4>
              <p class="text-neutral-600 dark:text-neutral-400 text-sm leading-relaxed transition-colors">Desde reportes automatizados hasta gestión de usuarios compleja. El sistema trabaja para ti y fluye como tu negocio lo necesita.</p>
            </div>
          </li>
          <li class="flex items-start gap-4 group/item">
            <div class="mt-1 p-2 bg-white dark:bg-neutral-950 rounded-lg border border-neutral-200 dark:border-neutral-800"><Users class="size-4 text-neutral-600 dark:text-neutral-300" /></div>
            <div>
              <h4 class="text-neutral-800 dark:text-neutral-100 font-medium mb-2 transition-colors">Usuarios ilimitados</h4>
              <p class="text-neutral-600 dark:text-neutral-400 text-sm leading-relaxed transition-colors">Crece todo lo que quieras. Al ser dueño del software, no pagas licencias extra por cada nuevo miembro que se une a tu equipo.</p>
            </div>
          </li>
          <li class="flex items-start gap-4 group/item">
            <div class="mt-1 p-2 bg-white dark:bg-neutral-950 rounded-lg border border-neutral-200 dark:border-neutral-800"><Boxes class="size-4 text-neutral-600 dark:text-neutral-300" /></div>
            <div>
              <h4 class="text-neutral-800 dark:text-neutral-100 font-medium mb-2 transition-colors">Todo centralizado</h4>
              <p class="text-neutral-600 dark:text-neutral-400 text-sm leading-relaxed transition-colors">Una sola plataforma unificada que integra todos tus procesos. Información en tiempo real, segura y sin fricciones operativas.</p>
            </div>
          </li>
        </ul>
      </div>
      </div>
    </div>
  </section>
</template>
