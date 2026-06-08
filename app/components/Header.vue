<script setup lang="ts">
import { Button } from '~/components/ui/button'
import { Aperture, Sun, Moon } from '@lucide/vue'

const colorMode = useColorMode()

const toggleTheme = () => {
  colorMode.preference = colorMode.value === 'dark' ? 'light' : 'dark'
}
</script>

<template>
  <header class="fixed top-4 inset-x-0 mx-auto w-full max-w-3xl z-50 px-4">
    <div class="flex items-center justify-between px-4 py-2 rounded-full bg-white/70 dark:bg-neutral-900/60 backdrop-blur-md border border-neutral-200 dark:border-neutral-700/50 text-neutral-900 dark:text-white shadow-xl transition-colors duration-300">
      
      <!-- Logo -->
      <NuxtLink to="/" class="flex items-center gap-2 font-semibold text-base hover:text-neutral-600 dark:hover:text-white transition-colors">
        <Aperture class="size-4 text-neutral-600 dark:text-neutral-300" />
        <span>rauantodev</span>
      </NuxtLink>

      <!-- Navigation Links -->
      <nav class="hidden md:flex items-center gap-4 text-sm font-medium text-neutral-600 dark:text-neutral-300">
        <NuxtLink to="#servicios" class="hover:text-neutral-900 dark:hover:text-white transition-colors">Servicios</NuxtLink>
        <NuxtLink to="#nosotros" class="hover:text-neutral-900 dark:hover:text-white transition-colors">Sobre mi</NuxtLink>
        <NuxtLink to="#proyectos" class="hover:text-neutral-900 dark:hover:text-white transition-colors">Proyectos</NuxtLink>
        <NuxtLink to="#contacto" class="hover:text-neutral-900 dark:hover:text-white transition-colors">Contacto</NuxtLink>
      </nav>

      <div class="flex items-center gap-3">
        <!-- Theme Toggle -->
        <button 
          @click="toggleTheme" 
          aria-label="Toggle Theme"
          class="flex items-center justify-center w-8 h-8 rounded-full bg-neutral-100 dark:bg-neutral-800 text-neutral-600 dark:text-neutral-300 hover:bg-neutral-200 dark:hover:bg-neutral-700 hover:text-neutral-900 dark:hover:text-white transition-colors border border-neutral-200 dark:border-neutral-700"
        >
          <ClientOnly>
            <Sun v-if="colorMode.value === 'dark'" class="w-4 h-4" />
            <Moon v-else class="w-4 h-4" />
            <template #fallback>
              <div class="w-4 h-4 rounded-full bg-neutral-300 dark:bg-neutral-600 animate-pulse"></div>
            </template>
          </ClientOnly>
        </button>

        <!-- CTA Button -->
        <Button variant="secondary" class="rounded-full bg-neutral-100 dark:bg-neutral-800 text-neutral-900 dark:text-white hover:bg-neutral-200 dark:hover:bg-neutral-700 border border-neutral-200 dark:border-neutral-700 h-8 px-3 text-xs font-medium transition-colors">
          Agenda una llamada
        </Button>
      </div>

    </div>
  </header>
</template>
