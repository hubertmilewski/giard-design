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

    <!-- Naglowek -->
    <h2
      :class="[
        'font-heading font-medium text-[28px] sm:text-[32px] md:text-[40px] lg:text-[48px] leading-tight',
        titleClass,
      ]"
    >
      <slot name="title" />
    </h2>

    <!-- Opis -->
    <p :class="['text-[16px]', descriptionClass]">
      <slot name="description" />
    </p>

    <!-- Przyciski CTA -->
    <div v-if="$slots.actions" class="flex flex-wrap gap-9 mt-6 lg:mt-18">
      <slot name="actions" />
    </div>
  </div>
</template>
