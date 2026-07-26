<script setup lang="ts">
defineProps<{
  tag?: string
  titleClass?: string
  descriptionClass?: string
}>()
</script>

<template>
  <div class="flex flex-col items-start">
    <!-- mała etykieta (np. "O firmie"), ale nie wymagana -->
    <span v-if="tag || $slots.tag" class="text-xs mb-4">
      <slot name="tag">{{ tag }}</slot>
    </span>

    <!-- Nagłówek (używamy slotu, żeby móc wstawić np. <em> do kursywy) -->
    <h2 :class="['font-heading font-medium tracking-tight', titleClass]">
      <slot name="title" />
    </h2>

    <!-- Opis -->
    <p :class="['text-base leading-relaxed opacity-90 max-w-xl text-[16px]', descriptionClass]">
      <slot name="description" />
    </p>

    <!-- Przyciski CTA -->
    <div class="flex flex-wrap gap-9 mt-18">
      <slot name="actions" />
    </div>
  </div>
</template>
