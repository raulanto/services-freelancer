<script setup lang="ts">
import { LayoutDashboard, LineChart, Wrench, Rocket } from '@lucide/vue'
import { onMounted, onUnmounted, ref } from 'vue'
import gsap from 'gsap'

const sectionRef = ref(null)
const itemsRef = ref([])

onMounted(() => {
  const ctx = gsap.context(() => {
    // Animar cada servicio a medida que se hace scroll
    itemsRef.value.forEach((item, index) => {
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
  }, sectionRef.value)

  onUnmounted(() => {
    ctx.revert()
  })
})
</script>

<template>
  <section ref="sectionRef" class="relative w-full py-40 bg-neutral-950 overflow-hidden">
    <!-- Fondo decorativo -->
    <div class="absolute inset-0 bg-[radial-gradient(ellipse_at_center,rgba(255,255,255,0.02)_0%,transparent_70%)] pointer-events-none"></div>

    <div class="max-w-6xl mx-auto px-4 relative z-10">
      <div class="text-center mb-32">
        <h2 class="text-4xl md:text-6xl font-serif text-neutral-100 mb-6 tracking-tight">
          Servicios <span class="italic text-neutral-400">Especializados</span>
        </h2>
        <p class="text-lg md:text-xl text-neutral-400 max-w-2xl mx-auto font-light">
          Soluciones tecnológicas diseñadas para aportar valor real, automatizar operaciones y asegurar la escalabilidad de tu negocio.
        </p>
      </div>

      <div class="space-y-32 md:space-y-40">
        
        <!-- Service 1: Web Core -->
        <div :ref="el => itemsRef.push(el)" class="flex flex-col md:flex-row items-center gap-12 md:gap-24 group">
          <div class="w-full md:w-1/2">
            <div class="w-20 h-20 bg-neutral-900 border border-neutral-800 rounded-3xl flex items-center justify-center mb-8 shadow-2xl group-hover:border-neutral-600 transition-colors duration-500">
              <LayoutDashboard class="size-8 text-neutral-300" />
            </div>
            <h3 class="text-3xl md:text-4xl font-medium text-white mb-6 leading-tight">
              Desarrollo de Aplicaciones Web Core
            </h3>
            <p class="text-lg text-neutral-400 leading-relaxed">
              Sistemas centrales diseñados específicamente para la gestión de tu negocio. Desde ERPs y CRMs a medida, hasta paneles de administración complejos que no te limitan a las reglas de un software empaquetado.
            </p>
          </div>
          <div class="w-full md:w-1/2 relative">
            <div class="absolute inset-0 bg-gradient-to-tr from-neutral-800/20 to-transparent rounded-[2rem] blur-2xl group-hover:bg-neutral-700/20 transition-all duration-700"></div>
            <div class="aspect-video bg-neutral-900/50 border border-neutral-800/80 rounded-[2rem] overflow-hidden backdrop-blur-sm relative z-10 shadow-2xl p-6 flex flex-col gap-4">
                <!-- Abstract UI representation -->
                <div class="flex gap-4 items-center border-b border-neutral-800 pb-4">
                    <div class="w-8 h-8 rounded-full bg-neutral-800"></div>
                    <div class="w-32 h-4 rounded-full bg-neutral-800"></div>
                </div>
                <div class="flex gap-4 h-full">
                    <div class="w-1/4 h-full bg-neutral-800/50 rounded-xl"></div>
                    <div class="w-3/4 flex flex-col gap-4">
                        <div class="w-full h-24 bg-neutral-800/50 rounded-xl"></div>
                        <div class="w-full flex-1 bg-neutral-800/50 rounded-xl"></div>
                    </div>
                </div>
            </div>
          </div>
        </div>

        <!-- Service 2: Automatización y Análisis -->
        <div :ref="el => itemsRef.push(el)" class="flex flex-col md:flex-row-reverse items-center gap-12 md:gap-24 group">
          <div class="w-full md:w-1/2">
            <div class="w-20 h-20 bg-neutral-900 border border-neutral-800 rounded-3xl flex items-center justify-center mb-8 shadow-2xl group-hover:border-neutral-600 transition-colors duration-500">
              <LineChart class="size-8 text-neutral-300" />
            </div>
            <h3 class="text-3xl md:text-4xl font-medium text-white mb-6 leading-tight">
              Automatización y Análisis de Datos
            </h3>
            <p class="text-lg text-neutral-400 leading-relaxed">
              Integración de procesos complejos que te hacen perder tiempo. Implementamos reportes históricos automatizados y cuadros de mando (dashboards) dinámicos para que tomes decisiones basadas en datos reales.
            </p>
          </div>
          <div class="w-full md:w-1/2 relative">
             <div class="absolute inset-0 bg-gradient-to-bl from-neutral-800/20 to-transparent rounded-[2rem] blur-2xl group-hover:bg-neutral-700/20 transition-all duration-700"></div>
             <div class="aspect-video bg-neutral-900/50 border border-neutral-800/80 rounded-[2rem] overflow-hidden backdrop-blur-sm relative z-10 shadow-2xl p-6 flex flex-col gap-4 justify-end">
                <!-- Abstract Charts -->
                <div class="flex items-end gap-3 h-full px-4">
                    <div class="w-full bg-neutral-800 rounded-t-lg h-[40%] group-hover:h-[60%] transition-all duration-1000"></div>
                    <div class="w-full bg-neutral-700 rounded-t-lg h-[60%] group-hover:h-[80%] transition-all duration-1000 delay-100"></div>
                    <div class="w-full bg-neutral-600 rounded-t-lg h-[30%] group-hover:h-[50%] transition-all duration-1000 delay-200"></div>
                    <div class="w-full bg-neutral-500 rounded-t-lg h-[80%] group-hover:h-[95%] transition-all duration-1000 delay-300"></div>
                    <div class="w-full bg-neutral-800 rounded-t-lg h-[50%] group-hover:h-[70%] transition-all duration-1000 delay-400"></div>
                </div>
             </div>
          </div>
        </div>

        <!-- Service 3: Landing Pages -->
        <div :ref="el => itemsRef.push(el)" class="flex flex-col md:flex-row items-center gap-12 md:gap-24 group">
          <div class="w-full md:w-1/2">
            <div class="w-20 h-20 bg-neutral-900 border border-neutral-800 rounded-3xl flex items-center justify-center mb-8 shadow-2xl group-hover:border-neutral-600 transition-colors duration-500">
              <Rocket class="size-8 text-neutral-300" />
            </div>
            <h3 class="text-3xl md:text-4xl font-medium text-white mb-6 leading-tight">
              Creación de Landing Pages de Alto Impacto
            </h3>
            <p class="text-lg text-neutral-400 leading-relaxed">
              Páginas de aterrizaje diseñadas estratégicamente para convertir. Utilizamos diseños premium, animaciones fluidas e integración profunda con tus herramientas de marketing para maximizar tus campañas.
            </p>
          </div>
          <div class="w-full md:w-1/2 relative">
             <div class="absolute inset-0 bg-gradient-to-br from-neutral-800/20 to-transparent rounded-[2rem] blur-2xl group-hover:bg-neutral-700/20 transition-all duration-700"></div>
             <div class="aspect-video bg-neutral-900/50 border border-neutral-800/80 rounded-[2rem] overflow-hidden backdrop-blur-sm relative z-10 shadow-2xl p-6 flex flex-col gap-4">
                <!-- Abstract Web Page -->
                <div class="w-full h-12 bg-neutral-800/50 rounded-xl mb-4 flex items-center px-4 justify-between">
                    <div class="w-16 h-4 bg-neutral-700 rounded-full"></div>
                    <div class="flex gap-2"><div class="w-8 h-2 bg-neutral-700 rounded-full"></div><div class="w-8 h-2 bg-neutral-700 rounded-full"></div></div>
                </div>
                <div class="flex-1 w-full bg-neutral-800/30 rounded-xl flex items-center justify-center">
                    <div class="w-1/2 h-8 bg-neutral-700 rounded-full group-hover:scale-110 transition-transform duration-700"></div>
                </div>
             </div>
          </div>
        </div>

        <!-- Service 4: Soporte -->
        <div :ref="el => itemsRef.push(el)" class="flex flex-col md:flex-row-reverse items-center gap-12 md:gap-24 group">
          <div class="w-full md:w-1/2">
            <div class="w-20 h-20 bg-neutral-900 border border-neutral-800 rounded-3xl flex items-center justify-center mb-8 shadow-2xl group-hover:border-neutral-600 transition-colors duration-500">
              <Wrench class="size-8 text-neutral-300" />
            </div>
            <h3 class="text-3xl md:text-4xl font-medium text-white mb-6 leading-tight">
              Evolución y Soporte a Largo Plazo
            </h3>
            <p class="text-lg text-neutral-400 leading-relaxed">
              No entrego el código para luego desaparecer. Mantengo y escalo la arquitectura conforme tu negocio crece, garantizando seguridad, rendimiento constante y adaptación a nuevas necesidades operativas.
            </p>
          </div>
          <div class="w-full md:w-1/2 relative">
             <div class="absolute inset-0 bg-gradient-to-tr from-neutral-800/20 to-transparent rounded-[2rem] blur-2xl group-hover:bg-neutral-700/20 transition-all duration-700"></div>
             <div class="aspect-video bg-neutral-900/50 border border-neutral-800/80 rounded-[2rem] overflow-hidden backdrop-blur-sm relative z-10 shadow-2xl p-6 flex items-center justify-center">
                <!-- Abstract Infinity / Loop -->
                <div class="relative w-32 h-32 flex items-center justify-center">
                    <div class="absolute w-full h-full border-4 border-dashed border-neutral-700 rounded-full group-hover:rotate-180 transition-transform duration-[2000ms] ease-in-out"></div>
                    <div class="absolute w-2/3 h-2/3 border-4 border-dashed border-neutral-600 rounded-full group-hover:-rotate-180 transition-transform duration-[2000ms] ease-in-out"></div>
                    <div class="w-1/3 h-1/3 bg-neutral-500 rounded-full group-hover:scale-125 transition-transform duration-1000"></div>
                </div>
             </div>
          </div>
        </div>

      </div>
    </div>
  </section>
</template>
