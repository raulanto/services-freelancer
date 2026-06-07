<script setup lang="ts">
import { Button } from '~/components/ui/button'
import Header from '~/components/Header.vue'
import LightRays from '~/components/LightRays/LightRays.vue'
import WhyCustom from '~/components/WhyCustom.vue'
import { onMounted, onUnmounted, ref } from 'vue'
import gsap from 'gsap'

const heroSection = ref(null)
const heroBg = ref(null)
const heroContent = ref(null)
let ctx: gsap.Context

onMounted(() => {
  ctx = gsap.context(() => {
    // Parallax para la imagen de fondo
    gsap.to(heroBg.value, {
      yPercent: 30,
      ease: 'none',
      scrollTrigger: {
        trigger: heroSection.value,
        start: 'top top',
        end: 'bottom top',
        scrub: true
      }
    })
    
    // Parallax y desvanecimiento para el contenido principal
    gsap.to(heroContent.value, {
      yPercent: 50,
      opacity: 0,
      ease: 'none',
      scrollTrigger: {
        trigger: heroSection.value,
        start: 'top top',
        end: 'bottom top',
        scrub: true
      }
    })
  })
})

onUnmounted(() => {
  ctx?.revert()
})
</script>

<template>
  <div class="w-full bg-neutral-950 font-sans text-neutral-200 min-h-screen">
    
    <!-- Header -->
    <Header />

    <!-- Hero Section -->
    <section ref="heroSection" class="relative min-h-screen w-full overflow-hidden flex flex-col items-center justify-center">
        <!-- LightRays Component at the top -->
        <div class="absolute top-0 left-0 w-full h-full pointer-events-none z-0 mix-blend-screen opacity-70">
            <ClientOnly>
                <LightRays
                rays-origin="top-center"
                :rays-speed="1.5"
                :light-spread="0.8"
                :ray-length="3"
                :follow-mouse="false"
                :mouse-influence="0.1"
                :noise-amount="0.1"
                :distortion="0.05"
                class-name="custom-rays"
                />
            </ClientOnly>
        </div>

        <!-- Background Image -->
        <div ref="heroBg" class="absolute inset-0 w-full h-full z-0 scale-110">
            <img src="~/assets/img/HJ43YeJboAAPOH6.jpeg" alt="Background"
                class="w-full h-full object-cover mix-blend-screen opacity-60" />
            <div class="absolute inset-0 bg-gradient-to-b from-transparent via-neutral-950/50 to-neutral-950 pointer-events-none"></div>
        </div>

        <!-- Hero Content -->
        <main ref="heroContent" class="relative z-10 text-center px-4 max-w-4xl mt-32">
            <h1 class="text-6xl md:text-8xl font-serif text-neutral-100 tracking-tight leading-[1.1] mb-8 font-medium">
                Código a medida para <span class="italic text-neutral-300">negocios que escalan.</span>
            </h1>
            <p class="text-xl md:text-2xl text-neutral-100 mb-12 font-light max-w-2xl mx-auto">
                Automatiza procesos y eleva tu presencia digital <br /> con desarrollo de software profesional.
            </p>

            <Button size="lg" variant="secondary"
                class="rounded-full bg-neutral-800/80 backdrop-blur-sm text-neutral-200 hover:bg-neutral-700 hover:text-white border border-neutral-700/50 px-8 py-6 text-lg font-medium transition-all">
                Comenzar proyecto
            </Button>
        </main>
    </section>

    <!-- Why Custom Section -->
    <WhyCustom />

  </div>
</template>