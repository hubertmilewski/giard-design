<script setup lang="ts">
const props = defineProps<{
  primaryHref?: string
  secondaryHref?: string
}>()

const primaryHref = props.primaryHref ?? '#'
const secondaryHref = props.secondaryHref ?? '#'
</script>

<template>
  <div class="flex flex-wrap items-center gap-6">
    <a
      v-if="$slots.primary"
      :href="primaryHref"
      class="inline-flex items-center justify-center rounded-full bg-primary px-6 py-3 text-sm text-base transition duration-200 hover:bg-primary/90 hover:shadow-sm"
    >
      <slot name="primary" />
    </a>

    <a
      v-if="$slots['primary-base']"
      :href="primaryHref"
      class="inline-flex items-center justify-center rounded-full bg-base px-6 py-3 text-sm text-primary transition duration-200 hover:bg-base/90 hover:shadow-sm"
    >
      <slot name="primary-base" />
    </a>

    <a
      v-if="$slots.secondary"
      :href="secondaryHref"
      class="inline-flex items-center justify-center rounded-full border border-primary px-6 py-3 text-sm text-primary transition duration-200 hover:bg-primary/10"
    >
      <slot name="secondary" />
    </a>

    <a
      v-if="$slots['secondary-base']"
      :href="secondaryHref"
      class="inline-flex items-center justify-center rounded-full border border-base px-6 py-3 text-sm text-base transition duration-200 hover:bg-base/10"
    >
      <slot name="secondary-base" />
    </a>
  </div>
</template>
