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
      class="inline-flex items-center justify-center rounded-full bg-primary px-6 py-3 text-sm text-white transition duration-200 hover:bg-primary/90 hover:shadow-sm"
    >
      <slot name="primary" />
    </a>

    <a
      v-if="$slots['primary-white']"
      :href="primaryHref"
      class="inline-flex items-center justify-center rounded-full bg-white px-6 py-3 text-sm text-primary transition duration-200 hover:bg-white/90 hover:shadow-sm"
    >
      <slot name="primary-white" />
    </a>

    <a
      v-if="$slots.secondary"
      :href="secondaryHref"
      class="inline-flex items-center justify-center rounded-full border border-primary px-6 py-3 text-sm text-primary transition duration-200 hover:bg-primary/10"
    >
      <slot name="secondary" />
    </a>

    <a
      v-if="$slots['secondary-white']"
      :href="secondaryHref"
      class="inline-flex items-center justify-center rounded-full border border-white px-6 py-3 text-sm text-white transition duration-200 hover:bg-white/10"
    >
      <slot name="secondary-white" />
    </a>
  </div>
</template>
