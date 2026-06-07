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
  ctx = gsap.context(() => {
    // Parallax background
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

    // Float-up animation for cards with different scrub speeds
    gsap.from(card1Ref.value, {
      y: 100,
      opacity: 0,
      ease: 'none',
      scrollTrigger: {
        trigger: sectionRef.value,
        start: 'top 80%',
        end: 'top 30%',
        scrub: 1
      }
    })

    gsap.from(card2Ref.value, {
      y: 150,
      opacity: 0,
      ease: 'none',
      scrollTrigger: {
        trigger: sectionRef.value,
        start: 'top 80%',
        end: 'top 30%',
        scrub: 1.5
      }
    })
  })
})

onUnmounted(() => {
  ctx?.revert()
})
</script>

<template>
  <section ref="sectionRef" class="relative w-full py-32 overflow-hidden h-screen">
    <!-- Skibg Background -->
    <div ref="bgRef" class="absolute inset-0 w-full h-full z-0 pointer-events-none scale-[1.2] -top-[10%]">
      <ClientOnly>
        <Skibg 
        />
      </ClientOnly>
    </div>

    <div class="w-full max-w-5xl mx-auto px-4 relative z-10 mt-40">
      <div class="text-center mb-20">
      <h2 class="text-4xl md:text-6xl font-serif text-neutral-100 mb-6 tracking-tight">
        El problema del <span class="italic text-neutral-400">software genérico</span>
      </h2>
      <p class="text-lg md:text-xl text-neutral-400 max-w-2xl mx-auto font-light">
        La mayoría de empresas dudan entre pagar suscripciones mensuales o construir un sistema propio. Descubre por qué un ecosistema a medida es la clave para escalar.
      </p>
    </div>

    <div class="grid md:grid-cols-2 gap-8 items-stretch">
      <!-- El Problema: Software Genérico -->
      <div ref="card1Ref" class="bg-neutral-900/40 border border-neutral-800/80 rounded-3xl p-10 backdrop-blur-md relative overflow-hidden group transition-colors hover:bg-neutral-900/60">
        <div class="flex items-center gap-4 mb-10">
          <div class="p-3 bg-neutral-950 rounded-2xl border border-neutral-800">
            <XCircle class="size-6 text-red-400/80" />
          </div>
          <h3 class="text-2xl font-medium text-neutral-300">SaaS Genérico</h3>
        </div>
        
        <ul class="space-y-8">
          <li class="flex items-start gap-5">
            <div class="mt-1 p-2 bg-neutral-950 rounded-lg"><Boxes class="size-4 text-neutral-500" /></div>
            <div>
              <h4 class="text-neutral-300 font-medium mb-2">Información fragmentada</h4>
              <p class="text-neutral-500 text-sm leading-relaxed">Terminas usando 5 herramientas distintas que no se comunican entre sí. Datos duplicados, procesos manuales y pérdida de tiempo.</p>
            </div>
          </li>
          <li class="flex items-start gap-5">
            <div class="mt-1 p-2 bg-neutral-950 rounded-lg"><Users class="size-4 text-neutral-500" /></div>
            <div>
              <h4 class="text-neutral-300 font-medium mb-2">Licencias por usuario</h4>
              <p class="text-neutral-500 text-sm leading-relaxed">Mientras más crece tu equipo, más cara se vuelve la suscripción mensual. Escalar se convierte en un coste fijo insostenible.</p>
            </div>
          </li>
          <li class="flex items-start gap-5">
            <div class="mt-1 p-2 bg-neutral-950 rounded-lg"><LineChart class="size-4 text-neutral-500" /></div>
            <div>
              <h4 class="text-neutral-300 font-medium mb-2">Limita tu crecimiento</h4>
              <p class="text-neutral-500 text-sm leading-relaxed">Debes adaptar la lógica de tu negocio a lo que el software permite, no al revés. Tus procesos únicos se pierden.</p>
            </div>
          </li>
        </ul>
      </div>

      <!-- La Solución: Software a Medida -->
      <div ref="card2Ref" class="bg-neutral-900/80 border border-neutral-700/60 rounded-3xl p-10 backdrop-blur-xl relative overflow-hidden shadow-[0_0_40px_-15px_rgba(255,255,255,0.1)] group transition-all hover:border-neutral-600">
        <div class="absolute inset-0 bg-gradient-to-br from-neutral-800/20 to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-500"></div>
        <div class="flex items-center gap-4 mb-10 relative z-10">
          <div class="p-3 bg-neutral-950 rounded-2xl border border-neutral-700">
            <CheckCircle class="size-6 text-neutral-200" />
          </div>
          <h3 class="text-2xl font-medium text-white">Ecosistema Propio</h3>
        </div>
        
        <ul class="space-y-8 relative z-10">
          <li class="flex items-start gap-5">
            <div class="mt-1 p-2 bg-neutral-950 rounded-lg border border-neutral-800"><ShieldCheck class="size-4 text-neutral-300" /></div>
            <div>
              <h4 class="text-neutral-100 font-medium mb-2">100% Adaptado a tu lógica</h4>
              <p class="text-neutral-400 text-sm leading-relaxed">Desde reportes automatizados hasta gestión de usuarios compleja. El sistema trabaja para ti y fluye como tu negocio lo necesita.</p>
            </div>
          </li>
          <li class="flex items-start gap-5">
            <div class="mt-1 p-2 bg-neutral-950 rounded-lg border border-neutral-800"><Users class="size-4 text-neutral-300" /></div>
            <div>
              <h4 class="text-neutral-100 font-medium mb-2">Usuarios ilimitados</h4>
              <p class="text-neutral-400 text-sm leading-relaxed">Crece todo lo que quieras. Al ser dueño del software, no pagas licencias extra por cada nuevo miembro que se une a tu equipo.</p>
            </div>
          </li>
          <li class="flex items-start gap-5">
            <div class="mt-1 p-2 bg-neutral-950 rounded-lg border border-neutral-800"><Boxes class="size-4 text-neutral-300" /></div>
            <div>
              <h4 class="text-neutral-100 font-medium mb-2">Todo centralizado</h4>
              <p class="text-neutral-400 text-sm leading-relaxed">Una sola plataforma unificada que integra todos tus procesos. Información en tiempo real, segura y sin fricciones operativas.</p>
            </div>
          </li>
        </ul>
      </div>
      </div>
    </div>
  </section>
</template>
