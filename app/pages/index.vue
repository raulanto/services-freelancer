<script setup lang="ts">
import Header from '~/components/Header.vue'
import LightRays from '~/components/LightRays/LightRays.vue'
import WhyCustom from '~/components/WhyCustom.vue'
import SpecializedServices from '~/components/SpecializedServices.vue'
import WorkMethodology from '~/components/WorkMethodology.vue'
import TechStack from '~/components/TechStack.vue'
import AboutUs from '~/components/AboutUs.vue'
import FAQ from '~/components/FAQ.vue'
import { onMounted, onUnmounted, ref } from 'vue'
import gsap from 'gsap'
import FooterPorta from '~/components/Footer.porta.vue'
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
  <div class="w-full bg-white dark:bg-neutral-950 font-sans text-neutral-900 dark:text-neutral-200 min-h-screen transition-colors duration-300">
    
    <!-- Header -->
    <Header />

    <!-- Hero Section -->
    <section ref="heroSection" class="relative min-h-screen w-full overflow-hidden flex flex-col items-center justify-center bg-neutral-950">
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
        </div>

        <!-- Hero Content: bottom-left layout inspired by Astrael -->
        <main ref="heroContent" class="absolute bottom-0 left-0 z-10 px-8 md:px-16 pb-16 md:pb-20 max-w-2xl text-left">

            <h1 class="text-5xl md:text-6xl lg:text-7xl font-italic font-bold font-serif  text-white leading-[1.08] mb-6">
                Código a medida para
                <em class="not-italic italic font-light text-white/60">negocios que escalan.</em>
            </h1>

            <p class="text-base md:text-lg text-white/50 mb-10 font-light leading-relaxed max-w-md">
                Automatiza procesos y eleva tu presencia digital con desarrollo de software profesional.
            </p>

            <div class="flex items-center gap-6">
                <!-- CTA principal con ícono de flecha -->
                <button class="group flex items-center gap-3 bg-white text-neutral-900 rounded-full pl-6 pr-2 py-2 text-sm font-semibold hover:bg-neutral-100 transition-all duration-300 shadow-lg">
                    Comenzar proyecto
                    <span class="flex items-center justify-center w-8 h-8 bg-neutral-900 rounded-full text-white group-hover:bg-neutral-800 transition-colors duration-300">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" class="w-3.5 h-3.5">
                            <path fill-rule="evenodd" d="M5.22 14.78a.75.75 0 0 0 1.06 0l7.22-7.22v5.69a.75.75 0 0 0 1.5 0v-7.5a.75.75 0 0 0-.75-.75h-7.5a.75.75 0 0 0 0 1.5h5.69l-7.22 7.22a.75.75 0 0 0 0 1.06Z" clip-rule="evenodd" />
                        </svg>
                    </span>
                </button>

                <!-- Enlace secundario estilo texto -->
                <button class="flex items-center gap-2 text-white/50 hover:text-white text-sm font-medium transition-colors duration-300 group">
                    Ver proyectos
                    <span class="transition-transform duration-300 group-hover:translate-x-1">→</span>
                </button>
            </div>

        </main>
    </section>

    <!-- Why Custom Section -->
    <WhyCustom />


    <!-- About Us Section -->
    <AboutUs />

    <!-- Specialized Services Section -->
    <SpecializedServices id="servicios" />

    <!-- Work Methodology Section -->
    <WorkMethodology />



    <!-- Tech Stack Section -->
    <TechStack />

    <!-- FAQ Section -->
    <FAQ />

  </div>

  <FooterPorta ></FooterPorta>  
</template>