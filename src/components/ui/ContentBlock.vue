<script setup lang="ts">
defineProps<{
  tag?: string
  titleClass?: string
}>()
</script>

<template>
  <div class="flex flex-col items-start max-w-149.75">
    <!-- mała etykieta (np. "O firmie"), ale nie wymagana -->
    <span v-if="tag" class="text-xs uppercase tracking-wider mb-4 opacity-80">
      {{ tag }}
    </span>

    <!-- Nagłówek (używamy slotu, żeby móc wstawić np. <em> do kursywy) -->
    <h2 :class="['font-heading font-medium tracking-tight', titleClass]">
      <slot name="title" />
    </h2>

    <!-- Opis -->
    <p class="mt-11 text-base leading-relaxed opacity-90 max-w-xl text-[16px]">
      <slot name="description" />
    </p>

    <!-- Przyciski CTA -->
    <div class="flex flex-wrap gap-9 mt-18">
      <slot name="actions" />
    </div>
  </div>
</template>
