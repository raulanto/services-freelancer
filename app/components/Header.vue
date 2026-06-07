<script setup lang="ts">
import { Button } from '~/components/ui/button'
import { Aperture } from '@lucide/vue'
</script>

<template>
  <header class="fixed top-6 inset-x-0 mx-auto w-full max-w-3xl z-50 px-4">
    <div class="flex items-center justify-between px-6 py-2.5 rounded-full bg-neutral-900/60 backdrop-blur-md border border-neutral-700/50 text-white shadow-xl">
      
      <!-- Logo -->
      <NuxtLink to="/" class="flex items-center gap-2 font-semibold text-lg hover:text-white transition-colors">
        <Aperture class="size-5 text-neutral-300" />
        <span>rauantodev</span>
      </NuxtLink>

      <!-- Navigation Links -->
      <nav class="hidden md:flex items-center gap-6 text-sm font-medium text-neutral-300">
        <NuxtLink to="#" class="hover:text-white transition-colors">Servicios</NuxtLink>
        <NuxtLink to="#" class="hover:text-white transition-colors">Sobre mi</NuxtLink>
        <NuxtLink to="#" class="hover:text-white transition-colors">Proyectos</NuxtLink>
        <NuxtLink to="#" class="hover:text-white transition-colors">Contacto</NuxtLink>
      </nav>

      <!-- CTA Button -->
      <Button variant="secondary" class="rounded-full bg-neutral-800 text-white hover:bg-neutral-700 border border-neutral-700 h-9 px-4 font-medium transition-colors">
        Agenda una llamada
      </Button>

    </div>
  </header>
</template>
