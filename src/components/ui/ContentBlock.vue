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
    <span v-if="tag || $slots.tag" class="text-primary text-[12px] mb-4">
      <slot name="tag">{{ tag }}</slot>
    </span>

    <!-- Nagłówek (używamy slotu, żeby móc wstawić np. <em> do kursywy) -->
    <h2 :class="['font-heading font-medium text-[48px]', titleClass]">
      <slot name="title" />
    </h2>

    <!-- Opis -->
    <p :class="['text-base text-[16px]', descriptionClass]">
      <slot name="description" />
    </p>

    <!-- Przyciski CTA -->
    <div v-if="$slots.actions" class="flex flex-wrap gap-9 mt-18">
      <slot name="actions" />
    </div>
  </div>
</template>
